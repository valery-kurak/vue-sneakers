<script setup>
import { ref, watch, provide, computed } from 'vue';

import Header from './components/Header.vue';
import Drawer from './components/Drawer.vue';

/* Basket (Start) */
const cart = ref([]);

const drawerOpen = ref(false);

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0));
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));

const bodyHTMLElement = document.body;

const closeDrawer = () => {
  drawerOpen.value = false;
  bodyHTMLElement.classList.remove('overflow-hidden');
}

const openDrawer = () => {
  drawerOpen.value = true;
  bodyHTMLElement.classList.add('overflow-hidden');
}

const addToCart = (item) => {
  cart.value.push(item);
  item.isAdded = true;
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1);
  item.isAdded = false;
}

watch(cart, () => {
  localStorage.setItem('cart', JSON.stringify(cart.value));
}, {
  deep: true
});

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
});
/* Basket (End) */
</script>

<template>
  <div class="w-4/5 my-8 lg:my-14 mx-auto bg-white rounded-xl shadow-xl">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <Drawer 
      v-if="drawerOpen" 
      :total-price="totalPrice" 
      :vat-price="vatPrice"
    />
    <div class="px-6 lg:px-10 py-6">
      <RouterView />
    </div>
  </div>
</template>