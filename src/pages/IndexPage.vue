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
    <q-tabs
      v-model="tab"
      class="text-grey"
      active-color="white"
      indicator-color="primary"
      align="justify"
      narrow-indicator
    >
      <q-tab name="best" label="Best" />
      <q-tab name="meal" label="Meal" />
      <q-tab name="know" label="Known" />
    </q-tabs>
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
const filter = ref('');
const tab = ref('best');
</script>
