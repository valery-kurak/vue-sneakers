<script setup>
import { onMounted, ref, reactive, watch } from 'vue';
import axios from 'axios';

import CardList from '../components/CardList.vue';

const items = ref([]);
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const OnChangeSelect = event => {
  filters.sortBy = event.target.value;
}

const OnChangeSearchInput = event => {
  filters.searchQuery = event.target.value;
}

const fetchItems = async() => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(
      `https://d4bb5a17b45d712c.mokky.dev/items`, {
        params
      }
    );

    items.value = data;
  } catch (error) {
    console.log(error);
  }
}

onMounted(fetchItems);
watch(filters, fetchItems);
</script>

<template>
  <section class="grid gap-8 px-10 py-6">
      <div class="flex justify-between items-center gap-4">
        <p class="text-3xl font-bold break-word">All sneakers</p>
        <div class="flex gap-4">
          <select @change="OnChangeSelect" class="py-3 px-3 border rounded-lg outline-none cursor-pointer transition hover:border-gray-400">
            <option value="name">By name</option>
            <option value="price">By price (ascending)</option>
            <option value="-price">By price (descending)</option>
          </select>
          <div class="relative">
            <img src="/search.svg" alt="Search" class="absolute top-[calc(50%-0.5rem)] left-4 cursor-pointer" />
            <input
              @input="OnChangeSearchInput"
              type="text"
              placeholder="Search..."
              class="py-3 pr-4 pl-11 border rounded-lg outline-none cursor-pointer transition focus:border-gray-400" 
            />
          </div>
        </div>
      </div>
      <CardList :items="items" />
    </section>
</template>