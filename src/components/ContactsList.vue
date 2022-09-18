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
import { definitions } from 'app/types/supabase';
import { defineProps } from 'vue';

const props = defineProps<{
  contacts: definitions['facebook'][];
}>();
</script>
