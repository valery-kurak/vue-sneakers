<script setup>
import Card from './Card.vue';

defineProps({
  items: Array,
  isFavorites: Boolean
});

const emit = defineEmits(['addToFavorite', 'addToCart']);
</script>

<template>
  <ul class="grid 2xl:grid-cols-4 lg:grid-cols-3 md:grid-cols-2 gap-6 xl:gap-10" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title" 
      :price="item.price" 
      :imageUrl="item.imageUrl"
      :onClickFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
      :onClickAdd="isFavorites ? null : () => emit('addToCart', item)"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </ul>
</template>