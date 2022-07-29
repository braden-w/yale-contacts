<template>
  <q-select
    filled
    :model-value="model"
    use-input
    hide-selected
    fill-input
    input-debounce="0"
    :options="options"
    @filter="filterFn"
    @input-value="setModel"
    hint="Text autocomplete"
    style="width: 250px; padding-bottom: 32px"
  >
    <template v-slot:no-option>
      <q-item>
        <q-item-section class="text-grey"> No results </q-item-section>
      </q-item>
    </template>
  </q-select>
</template>
<script setup lang="ts">
import { supabase } from '../supabase';
import { definitions } from 'components/supabase';
import { ref } from 'vue';

// Select all contacts from "facebook" table whose "email", "first_name", or "last_name" is not null
const { data: allContacts, error } = await supabase
  .from<definitions['facebook']>('facebook')
  .select('email, first_name, last_name, image')
  .not('email', 'is', null)
  .not('first_name', 'is', null)
  .not('last_name', 'is', null);
console.log(allContacts);

const model = ref(null);
const options = ref(allContacts);
function filterFn(val, update, abort) {
  update(() => {
    const needle = val.toLocaleLowerCase();
    options.value = allContacts.filter(
      ({ email, first_name, last_name }) =>
        email.toLocaleLowerCase().indexOf(needle) > -1
    );
  });
}
function setModel(val) {
  model.value = val;
}
</script>
