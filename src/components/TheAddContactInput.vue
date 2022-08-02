<template>
  <q-select
    filled
    :model-value="selectedContact"
    use-input
    hide-selected
    fill-input
    input-debounce="0"
    :options="options"
    option-value="email"
    option-label="full_name"
    option-disable="inactive"
    @filter="filterFn"
    @input-value="setModel"
    hint="Text autocomplete"
    style="width: 250px; padding-bottom: 32px"
  >
    <template v-slot:no-option>
      <q-item>
        <q-item-section class="text-grey"> No results </q-item-section>
      </q-item>
    </template>
  </q-select>

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
import { ref } from 'vue';
import ContactsList from 'src/components/ContactsList.vue';

// Select all contacts from "facebook" table whose "email", "first_name", or "last_name" is not null
const { data: allContacts, error } = await supabase
  .from<definitions['facebook']>('facebook')
  .select('email, first_name, last_name, image')
  .not('email', 'is', null)
  .not('first_name', 'is', null)
  .not('last_name', 'is', null)
  .not('year', 'is', null)
  .eq('year', 2024);
console.log(allContacts);
// allContacts?.forEach((contact) => {
//   contact.full_name = `${contact.first_name} ${contact.last_name}`;
// });
const allOptions = allContacts?.map(
  ({ email, first_name, last_name, image }) => ({
    email,
    full_name: `${first_name} ${last_name}`,
    image,
  })
);

const selectedContact = ref(null);
const options = ref(allOptions);
function filterFn(val, update, abort) {
  update(() => {
    const needle = val.toLocaleLowerCase();
    options.value = allOptions.filter(
      ({ email, first_name, last_name }) =>
        email.toLocaleLowerCase().indexOf(needle) > -1
    );
  });
}
function setModel(val) {
  selectedContact.value = val;
}

async function addRelationshipToSupabase() {
  // Upsert relationship to "relationships_between_users"
  const { data, error } = await supabase
    .from<definitions['relationships_between_users']>(
      'relationships_between_users'
    )
    .insert({
      from_email: 'braden.wong@yale.edu',
      to_email: selectedContact.value.email,
    });

  if (error) console.error(error);
  else console.log(data);
}
</script>
