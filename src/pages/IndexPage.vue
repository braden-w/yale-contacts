<template>
  <q-page>
    <q-card class="q-mb-md">
      <TheAddContactInput />
      <q-input
        v-model="filter"
        standout
        dense
        debounce="300"
        placeholder="Filter contacts..."
        style="width: 100%"
      >
        <template #append>
          <q-icon name="search" />
        </template>
      </q-input>
    </q-card>
    <q-tabs v-model="tab" indicator-color="primary" align="justify">
      <q-tab name="best" label="Best" />
      <q-tab name="meal" label="Meal" />
      <q-tab name="know" label="Known" />
    </q-tabs>
    <q-separator />
    <q-tab-panels v-model="tab">
      <q-tab-panel name="best">
        <ContactsList :contacts="contacts" />
      </q-tab-panel>
      <q-tab-panel name="meal">
        <ContactsList :contacts="contacts" />
      </q-tab-panel>
      <q-tab-panel name="know">
        <ContactsList :contacts="contacts" />
      </q-tab-panel>
    </q-tab-panels>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { supabase } from '../supabase';
import { definitions } from 'app/types/supabase';
import ContactsList from '../components/ContactsList.vue';
import TheAddContactInput from '../components/TheAddContactInput.vue';

const filter = ref('');
const tab = ref('best');

// Import all contacts that are connected to "braden.wong@yale.edu" from relationships_between_users
const contacts = ref<definitions['facebook'][] | null>([]);

const fetchContacts = async (): Promise<definitions['facebook'][] | null> => {
  // Select all contacts from "users" table whose "email" is
  const { data, error } = await supabase
    .from('relationships_between_users')
    .select('facebook(*)')
    .eq('from_email', 'braden.wong@yale.edu');
  if (error) {
    console.error(error);
  }
  // Set contacts to the result of fetchContacts
  if (!data) return null;
  const facebook = data.map(({ facebook }) => facebook);
  contacts.value = facebook;
  return facebook;
};
fetchContacts();
</script>
