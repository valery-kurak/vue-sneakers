<script setup>
defineProps({
  id: Number,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  imageUrl: String,
  onClickFavorite: Function,
  onClickAdd: Function
})
</script>

<template>
  <li class="relative grid gap-2 p-8 bg-white border border-slate-100 rounded-3xl cursor-pointer transition hover:-translate-y-2 hover:shadow-md">
    <div class="grid gap-2">
      <button v-if="onClickFavorite" @click="onClickFavorite" class="absolute top-8 left-8 w-8 h-8" aria-label="Add to favorites">
        <img :src="!isFavorite ? 'like-1.svg' : '/like-2.svg'" alt="Like icon" />
      </button>
      <img :src="imageUrl" alt="Sneaker" class="w-full max-h-[12.875rem] object-contain" />
      <p class="break-word">{{ title }}</p>
    </div>
    <div class="flex justify-between gap-2 mt-auto">
      <div class="grid break-word">
        <span class="text-slate-400">Price: </span>
        <b>{{ price }} $</b>
      </div>
      <button v-if="onClickAdd" @click="onClickAdd" class="w-8 h-8" aria-label="Add to basket">
        <img :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="Add icon" />
      </button>
    </div>
  </li>
</template>