<template>
  <q-page class="row items-center justify-evenly">
    <example-component
      title="Example component"
      active
      :todos="todos"
      :meta="meta"
    ></example-component>
  </q-page>
</template>

<script setup lang="ts">
import { definitions } from 'components/supabase';
import { ref } from 'vue';

const contacts = ref<definitions['users'][]>([]);
</script>
