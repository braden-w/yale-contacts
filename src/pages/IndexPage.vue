<template>
  <q-page class="row items-center justify-evenly"> </q-page>
</template>

<script setup lang="ts">
import { supabase } from '../supabase';
import { definitions } from 'components/supabase';
import { ref } from 'vue';

// Import all contacts that are connected to "braden.wong@yale.edu"

const contacts = ref<definitions['users'][]>([]);
</script>
