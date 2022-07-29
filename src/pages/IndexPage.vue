<template>
  <q-page class="row items-center justify-evenly"> </q-page>
</template>

<script setup lang="ts">
import { supabase } from '../supabase';
import { definitions } from 'components/supabase';
import { ref } from 'vue';

// Import all contacts that are connected to "braden.wong@yale.edu" from relationships_between_users
const fetchContacts = async () => {
  const { data, error } = await supabase
    .from('relationships_between_users')
    .select('*')
    .eq('from_email', 'braden.wong@yale.edu');
  if (error) {
    console.error(error);
  }
  return data;
};

console.log(await fetchContacts());
const contacts = ref<definitions['users'][]>([]);
</script>
