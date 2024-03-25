<script setup>
import { inject, reactive, watch, ref, onMounted } from 'vue';
import axios from 'axios';
import debounce from 'lodash.debounce';

import CardList from '../components/CardList.vue';

const { cart, addToCart, removeFromCart } = inject('cart');

const items = ref([]);

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
});

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removeFromCart(item);
  }
}

const OnChangeSelect = event => {
  filters.sortBy = event.target.value;
}

const OnChangeSearchInput = debounce(event => {
  filters.searchQuery = event.target.value;
}, 200);

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
      item_id: item.id
    }
      item.isFavorite = true;
      const { data } = await axios.post('https://d4bb5a17b45d712c.mokky.dev/favorites', obj);
      item.favoriteId = data.id;
    } else {
      item.isFavorite = false;
      await axios.delete(`https://d4bb5a17b45d712c.mokky.dev/favorites/${item.favoriteId}`);
      item.favoriteId = null;
    }
  } catch (error) {
    console.log(error);
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://d4bb5a17b45d712c.mokky.dev/favorites');

    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.item_id === item.id);

      if (!favorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    });
  } catch (error) {
    console.log(error);
  }
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

    items.value = data.map(obj => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }));
  } catch (error) {
    console.log(error);
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart');
  cart.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }));
});

watch(filters, fetchItems);

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }));
});
</script>

<template>
  <section class="grid gap-8">
    <div class="flex flex-col lg:flex-row lg:justify-between lg:items-center gap-4">
      <p class="text-2xl lg:text-3xl font-bold break-word">All sneakers</p>
      <div class="flex flex-col sm:flex-row gap-4">
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
            class="w-full py-3 pr-4 pl-11 border rounded-lg outline-none cursor-pointer transition focus:border-gray-400" 
          />
        </div>
      </div>
    </div>
    <CardList 
      :items="items" 
      @add-to-favorite="addToFavorite" 
      @add-to-cart="onClickAddPlus" 
    />
  </section>
</template>