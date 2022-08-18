<template>
  <q-select
    filled
    v-model="selectedContact"
    use-input
    hide-selected
    fill-input
    input-debounce="0"
    :options="options"
    option-value="email"
    option-label="name"
    option-disable="inactive"
    @filter="filterFn"
    @input-value="setModel"
    hint="Text autocomplete"
  >
    <template v-slot:no-option>
      <q-item>
        <q-item-section class="text-grey"> No results </q-item-section>
      </q-item>
    </template>
  </q-select>
  {{ selectedContact }}

  <q-btn
    @click="addRelationshipToSupabase"
    :disabled="!selectedContact"
    color="primary"
  >
    Submit
  </q-btn>
</template>
<script setup lang="ts">
import { supabase } from '../supabase';
import { definitions } from 'components/supabase';
import { Ref, ref } from 'vue';
import ContactsList from 'src/components/ContactsList.vue';

type query = definitions['users_contact_app'] & { email: string };
const { data: allContacts } = await supabase
  .from<query>('users_contact_app')
  .select('*');

const selectedContact: Ref<definitions['users_contact_app'] | null> = ref(null);
const setModel = (val) => (selectedContact.value = val);

const options = ref(allContacts);
function filterFn(val, update, abort) {
  update(() => {
    if (!allContacts) return;
    const needle = val.toLocaleLowerCase();
    options.value = allContacts.filter(
      ({ email }) => email.toLocaleLowerCase().indexOf(needle) > -1
    );
  });
}

async function addRelationshipToSupabase() {
  if (!selectedContact.value) return;
  // Upsert relationship to "relationships_between_users"
  const { data, error } = await supabase
    .from<definitions['relationships_between_users']>(
      'relationships_between_users'
    )
    .upsert({
      from_email: 'braden.wong@yale.edu',
      to_email: selectedContact.value.email,
    });

  if (error) console.error(error);
  else console.log(data);
}
</script>
