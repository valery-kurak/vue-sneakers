<script setup>
defineProps({
  imageUrl: String,
  title: String,
  description: String
})
</script>

<template>
  <article class="max-w-72 grid gap-3 justify-items-center m-auto text-center break-word">
    <img :src="imageUrl" alt="Info icon" class="max-w-32 max-h-32">
    <p class="text-2xl font-bold text-black">{{ title }}</p>
    <p class="text-base text-slate-400">{{ description }}</p>
  </article>
</template>