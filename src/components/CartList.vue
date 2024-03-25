<script setup>
import { inject } from 'vue';
import CartItem from './CartItem.vue';

const { cart, removeFromCart } = inject('cart');
</script>

<template>
  <ul class="grid gap-5" v-auto-animate>
    <CartItem 
      v-for="item in cart" 
      :key="item.id"
      :title="item.title" 
      :imageUrl="item.imageUrl"
      :price="item.price"
      @on-click-remove="() => removeFromCart(item)" 
    />
  </ul>
</template>