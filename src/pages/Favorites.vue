<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

import CardList from '../components/CardList.vue';
import InfoBlock from '../components/InfoBlock.vue';

const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get('https://d4bb5a17b45d712c.mokky.dev/favorites?_relations=items');
    favorites.value = data.map(obj => obj.item);
  } catch (error) {
    console.log(error);
  }
});
</script>

<template>
  <div v-if="favorites.length > 0" class="grid gap-8">
    <p class="text-3xl font-bold break-word">My favorites</p>
      <CardList :items="favorites" is-favorites />
  </div>
  <InfoBlock v-else image-url="/emoji-1.png" title="No bookmarks :(" description="You didn't bookmark anything." />
</template>