<script setup>
  import DrawerHead from '@/components/DrawerHead.vue'
  import CardItemList from '@/components/CardItemList.vue'
  import InfoBlock from '@/components/InfoBlock.vue'
  import axios from 'axios'
  import { ref, computed, inject } from 'vue'

  const props = defineProps({
    totalPrice: Number,
    vatPrice: Number,
  })

  const {
    cart,
  } = inject('cart')

  const isCreating = ref(false);
  const orderId = ref(null);


  const createOrder = async () => {
    try {
      isCreating.value = true;
      const { data } = await axios.post('https://cef2ecffb28791e3.mokky.dev/orders', {
        items: cart.value,
        totalPrice: props.totalPrice.value,
      })

      cart.value = [];

      orderId.value = data.id;
    } catch (error) {
      console.log(error)
    } finally {
      isCreating.value = false;
    }
  }

  const buttonDisabled = computed(() =>  isCreating.value || cartIsEmpty.value);
  const cartIsEmpty = computed(() => cart.value.length === 0);

</script>

<template>
  <div class="fixed top-0 left-0 w-full h-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead/>

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен!"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
      />
    </div>

    <div v-else>

    <CardItemList />
    <div class="flex flex-col gap-4 my-6">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }} rub</b>
      </div>

      <div class="flex gap-2">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ vatPrice }} rub</b>
      </div>

      <button
        :disabled="buttonDisabled"
        @click="createOrder"
        class="mt-7 cursor-pointer disabled:bg-slate-400 transition bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 active:bg-lime-700">
        Оформить заказ
      </button>

    </div>
  </div>
  </div>
</template>