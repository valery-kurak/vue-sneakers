<script setup>
import { ref, inject, computed } from 'vue';
import axios from 'axios';

import DrawerHeader from './DrawerHeader.vue';
import CartList from './CartList.vue';
import InfoBlock from './InfoBlock.vue';

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
});

const { cart } = inject('cart');

const isCreating = ref(false);
const orderId = ref(null);

const createOrder = async () => {
  try {
    isCreating.value = true;
    const { data } = await axios.post('https://d4bb5a17b45d712c.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    });

    cart.value = [];
    orderId.value = data.id;
  } catch (error) {
    console.log(error);
  } finally {
    isCreating.value = false;
  }
}

const isCartEmpty = computed(() => cart.value.length === 0);
const isCartButtonDisabled = computed(() => isCreating.value || isCartEmpty.value);
</script>

<template>
  <div class="fixed top-0 left-0 z-10 w-full h-full bg-black/70">
    <aside class="flex flex-col gap-7 w-full sm:w-96 h-full p-6 lg:p-8 float-right bg-white">
      <DrawerHeader />
      <InfoBlock 
        v-if="!totalPrice && !orderId" 
        image-url="/package-icon.png" 
        title="Basket is empty" 
        description="Add at least one pair of sneakers to place an order." 
      />
      <InfoBlock 
        v-if="orderId" 
        image-url="/order-success-icon.png" 
        title="Order is placed!" 
        :description="`Your order #${orderId} will soon be transferred to courier delivery.`" 
      />
      <CartList v-if="totalPrice" />
      <div v-if="totalPrice" class="grid gap-5 mt-auto">
        <div class="grid grid-cols-2 gap-3 break-word">
          <span>Total:</span>
          <b class="text-right">{{ totalPrice }} $</b>
        </div>
        <div class="grid grid-cols-2 gap-3 break-word">
          <span>VAT 5%:</span>
          <b class="text-right">{{ vatPrice }} $</b>
        </div>
        <button 
          @click="createOrder" 
          :disabled="isCartButtonDisabled" 
          class="py-4 px-5 font-bold text-white bg-lime-500 rounded-xl transition hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-300"
        >Checkout</button>
      </div>
    </aside>
  </div>
</template>