<script setup>
defineProps({
  totalPrice: Number
});

const emit = defineEmits(['openDrawer']);
</script>

<template>
  <header class="flex flex-col lg:flex-row justify-between gap-4 px-6 lg:px-10 py-6 border-b border-slate-200">
    <RouterLink to="/" class="flex items-center gap-4 lg:max-w-[50%]" aria-label="Home page">
      <img src="/logo.png" alt="Logo" class="w-10">
      <div>
        <p class="text-xl font-bold uppercase break-word">Vue Sneakers</p>
        <p class="text-slate-400 break-word">Best sneaker store</p>
      </div>
    </RouterLink>
    <ul class="flex flex-col lg:flex-row lg:items-center gap-3 lg:gap-10">
      <li>
        <button @click="() => emit('openDrawer')" class="flex items-center gap-3 text-gray-500 hover:text-black" aria-label="Cart">
          <img src="/cart.svg" alt="Cart icon" />
          <b class="break-word">{{ totalPrice }} $</b>
        </button>
      </li>
      <li>
        <RouterLink to="/favorites" class="flex items-center gap-3 text-gray-500 hover:text-black" aria-labelledby="bookmarksLabel">
          <img src="/heart.svg" alt="Bookmarks icon" />
          <span id="bookmarksLabel" class="break-word">Bookmarks</span>
        </RouterLink>
      </li>
      <li>
        <button class="flex items-center gap-3 text-gray-500 hover:text-black" aria-labelledby="profileLabel">
          <img src="/profile.svg" alt="Profile icon" />
          <span id="profileLabel" class="break-word">Profile</span>
        </button>
      </li>
    </ul>
  </header>
</template>