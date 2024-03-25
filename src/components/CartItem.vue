<script setup>
const emit = defineEmits(['onClickRemove']);

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
})
</script>

<template>
  <li class="flex items-center gap-5 p-5 border border-slate-100 rounded-xl">
    <img :src="imageUrl" :alt="title" class="w-16 h-16" />
    <div class="flex flex-col w-full">
      <p class="w-[calc(100%-2.75rem)] break-word">{{ title }}</p>
      <div class="flex justify-between items-end gap-3">
        <b class="break-word">{{ price }} $</b>
        <button @click="emit('onClickRemove')" class="w-8 h-8" aria-label="Remove from basket">
          <img src="/close.svg" alt="Remove" class="opacity-50 transition hover:opacity-100" />
        </button>
      </div>
    </div>
  </li>
</template>