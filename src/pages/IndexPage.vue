<template>
  <q-page>
    <q-card class="q-mb-md">
      <Suspense timeout="0">
        <template #default> <TheAddContactInput /></template>
        <template #fallback><div>Loading...</div></template>
      </Suspense>
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
import { ref } from 'vue';
import ContactsList from '../components/ContactsList.vue';
import TheAddContactInput from '../components/TheAddContactInput.vue';
const filter = ref<string>('');
</script>
