<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="yale-blue-1">
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-btn
          class="text-h6 text-weight-light"
          no-caps
          flat
          to="/"
          icon="breakfast_dining"
          label="Yale Buttery Book"
        />
        <q-avatar>
          <!-- TODO: <img src="../../public/Icon.png" alt="" /> -->
        </q-avatar>
        <!-- <q-btn
          v-if="$q.screen.gt.md"
          class="yale-accent-1"
          icon="download"
          label="Add to Home Screen"
          @click="installDialog.open"
        />
        <q-btn v-else round flat icon="download" @click="installDialog.open" /> -->
        <q-space />
        <ReportDialog />
        <InstallDialog />
        <q-btn round flat icon="info" to="/about" />
        <!-- <q-btn
          v-if="$q.screen.lt.sm"
          icon="search"
          color="accent"
          to="/menus"
        />
        <q-btn
          v-else-if="$q.screen.lt.md"
          icon="search"
          color="accent"
          label="Menus"
          to="/menus"
        />
        <q-btn
          v-else
          icon="search"
          color="accent"
          label="Browse Menus"
          to="/menus"
        /> -->

        <q-toggle
          color="blue-grey-5"
          v-model="$q.dark.mode"
          @click="$q.dark.toggle()"
        >
        </q-toggle>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header> Essential Links </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import EssentialLink, {
  EssentialLinkProps,
} from 'components/EssentialLink.vue';

const essentialLinks: EssentialLinkProps[] = [
  {
    title: 'Docs',
    caption: 'quasar.dev',
    icon: 'school',
    link: 'https://quasar.dev',
  },
  {
    title: 'Github',
    caption: 'github.com/quasarframework',
    icon: 'code',
    link: 'https://github.com/quasarframework',
  },
  {
    title: 'Discord Chat Channel',
    caption: 'chat.quasar.dev',
    icon: 'chat',
    link: 'https://chat.quasar.dev',
  },
  {
    title: 'Forum',
    caption: 'forum.quasar.dev',
    icon: 'record_voice_over',
    link: 'https://forum.quasar.dev',
  },
  {
    title: 'Twitter',
    caption: '@quasarframework',
    icon: 'rss_feed',
    link: 'https://twitter.quasar.dev',
  },
  {
    title: 'Facebook',
    caption: '@QuasarFramework',
    icon: 'public',
    link: 'https://facebook.quasar.dev',
  },
  {
    title: 'Quasar Awesome',
    caption: 'Community Quasar projects',
    icon: 'favorite',
    link: 'https://awesome.quasar.dev',
  },
];

const leftDrawerOpen = ref(false);

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}
</script>
