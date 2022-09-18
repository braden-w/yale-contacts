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
    <template v-slot:option="scope">
      <q-item v-bind="scope.itemProps">
        <q-item-section avatar>
          <q-img :src="scope.opt.avatar_url" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ scope.opt.name }}</q-item-label>
          <q-item-label caption>
            {{ scope.opt.college }} {{ scope.opt.year }}
          </q-item-label>
        </q-item-section>
      </q-item>
    </template>

    <template v-slot:no-option>
      <q-item>
        <q-item-section class="text-grey"> No results </q-item-section>
      </q-item>
    </template>
  </q-select>
  {{ selectedContact }}
  <q-avatar color="primary" text-color="white">
    <img
      v-if="selectedContact"
      :src="selectedContact.avatar_url"
      alt="Profile Picture"
    />
  </q-avatar>
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
import { definitions } from 'app/types/supabase';
import { Ref, ref } from 'vue';

type query = definitions['users_contact_app'];
const allContacts: Ref<query[]> = ref([]);
supabase
  .from<query>('users_contact_app')
  .select('*')
  .then(({ data }) => {
    if (data) allContacts.value = data;
  });

const selectedContact: Ref<definitions['users_contact_app'] | null> = ref(null);
const setModel = (val: definitions['users_contact_app'] | null) =>
  (selectedContact.value = val);

const options = ref(allContacts);
function filterFn(val: string, update: (arg0: () => void) => void) {
  update(() => {
    if (!allContacts.value) return;
    const needle = val.toLocaleLowerCase();
    options.value = allContacts.value.filter(({ email }) => {
      if (email) return email.toLocaleLowerCase().indexOf(needle) > -1;
    });
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
      strength: 1,
    });

  if (error) console.error(error);
  else console.log(data);
}
</script>
