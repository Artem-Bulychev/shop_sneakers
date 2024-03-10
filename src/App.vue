<script setup>

  import {  ref, watch, provide, computed } from 'vue'


  import Header from './components/Header.vue';
  import Drawer from './components/Drawer.vue';

// Корзина
  const cart = ref([]);

  const drawerOpen = ref(false);


  const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0));

  const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));
  const openDrawer = () => {
    drawerOpen.value = true;
    }

  const closeDrawer = () => {
    drawerOpen.value = false;
  }


  const addToCart = (item) => {
    cart.value.push(item);
    item.isAdded = true;
  }
  const removeFromCart = (item) => {
      cart.value.slice(cart.value.indexOf(item), 1)
      item.isAdded = false;
  }


  watch(cart, () => {
      localStorage.setItem('cart', JSON.stringify(cart.value))
    }, {deep: true}
  )

  provide('cart', {
    cart,
    closeDrawer,
    openDrawer,
    addToCart,
    removeFromCart,
  });

  // Корзина


</script>


<template>
  <Drawer v-if="drawerOpen"
          :total-price="totalPrice"
          :vat-price="vatPrice"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-2xl mt-12">
  <Header :total-price="totalPrice "  @openDrawer="openDrawer"/>

  <div class="p-10">
    <router-view></router-view>

  </div>
  </div>
</template>
