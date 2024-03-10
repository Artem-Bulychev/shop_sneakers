<script setup>

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function,
})

</script>

<template>
  <div>
    <div class="relative bg-white border-slate-100 rounded-2xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition">

      <img v-if="onClickFavorite"
           @click="onClickFavorite"
           class="absolute top-0 left-0"
           :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
           alt="Like-1">

      <img :src="imageUrl" alt="Sneakers" />
      <p class="mt-2"> {{ title }}</p>

      <div class="flex justify-between mt-5">
        <div class="flex flex-col">
          <span class="text-slate-400">Цена:</span>
          <span> {{ price }} rub.</span>
        </div>

        <img v-if="onClickAdd"
             @click="onClickAdd"
             :src="!isAdded ? '/plus.svg' : '/checked.svg'"
             alt="Plus">
      </div>
    </div>
  </div>

</template>