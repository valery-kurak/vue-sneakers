<script setup>
import { inject } from 'vue';
const { closeDrawer } = inject('cart');
</script>

<template>
  <div class="flex items-center gap-4">
    <button @click="closeDrawer">
      <svg 
        class="min-w-4 opacity-30 rotate-180 transition cursor-pointer hover:opacity-100" 
        width="16" height="14" viewBox="0 0 16 14" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M1 7H14.7143" stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M8.71436 1L14.7144 7L8.71436 13" stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>
    <p class="text-2xl font-bold break-word">Basket</p>
  </div>
</template>