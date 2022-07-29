<template>
  <q-list>
    <q-item v-for="contact in contacts" :key="contact.id">
      <q-item-section avatar>
        <q-avatar color="primary" text-color="white">
          <img
            v-if="contact.image"
            :src="contact.image"
            alt="Profile Picture"
          />
          <div v-else>{{ contact.email.charAt(0) }}</div>
        </q-avatar>
      </q-item-section>

      <q-item-section>
        <q-item-label>{{
          `${contact.first_name} ${contact.last_name}`
        }}</q-item-label>
        <q-item-label caption lines="1">{{ contact.email }}</q-item-label>
      </q-item-section>

      <q-item-section side>
        <q-icon name="chat_bubble" color="green" />
      </q-item-section>
    </q-item>
  </q-list>
</template>
<script setup lang="ts">
import { supabase } from '../supabase';
import { definitions } from 'components/supabase';
import { ref } from 'vue';

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
  return data.map(({ facebook }) => facebook);
};

contacts.value = await fetchContacts();
console.log(await fetchContacts());

// Select all contacts from "facebook" table whose "email", "first_name", or "last_name" is not null
const { data: allContacts, error } = await supabase
  .from<definitions['facebook']>('facebook')
  .select('email, first_name, last_name, image')
  .not('email', 'is', null)
  .not('first_name', 'is', null)
  .not('last_name', 'is', null);
console.log(allContacts);
</script>
