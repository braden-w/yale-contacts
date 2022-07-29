<template>
  <q-page>
    <q-card class="q-mb-md">
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
    <Suspense timeout="0">
      <!-- Make beautiful list of contact photos, name, college -->
      <template #default> <ContactsList /></template>
      <template #fallback><div>Loading...</div></template>
    </Suspense>
  </q-page>
</template>

<script setup lang="ts">
import { definitions } from 'src/components/supabase';
import { supabase } from 'src/supabase';
import { ref } from 'vue';
import ContactsList from '../components/ContactsList.vue';
const filter = ref<string>('');

// Select all contacts from "facebook" table whose "email", "first_name", or "last_name" is not null
const { data: allContacts, error } = await supabase
  .from<definitions['facebook']>('facebook')
  .select('email, first_name, last_name, image')
  .not('email', 'is', null)
  .not('first_name', 'is', null)
  .not('last_name', 'is', null);
console.log(allContacts);
</script>
