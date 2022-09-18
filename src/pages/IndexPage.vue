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
        <Suspense timeout="0">
          <!-- Make beautiful list of contact photos, name, college -->
          <template #default> <ContactsList /></template>
          <template #fallback><div>Loading...</div></template>
        </Suspense></q-tab-panel
      >
      <q-tab-panel name="meal"></q-tab-panel>
      <q-tab-panel name="know"></q-tab-panel>
    </q-tab-panels>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import ContactsList from '../components/ContactsList.vue';
import TheAddContactInput from '../components/TheAddContactInput.vue';
const filter = ref('');
const tab = ref('best');
</script>
