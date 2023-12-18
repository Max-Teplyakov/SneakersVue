<script setup>
defineProps({
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function
})
</script>

<template>
  <div
    class="border bg-white solide rounded-2xl relative p-8 m-3 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
  >
    <img
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Лайк 1"
      class="absolute top-4 left-4"
      @click="onClickFavorite"
    />
    <img :src="imageUrl" :alt="title" />
    <p class="text-l font-bold">{{ title }}</p>
    <div class="flex justify-between">
      <p>
        Цена: <span class="font-bold">{{ price }}</span>
      </p>
      <img :src="!isAdded ? '/plus.svg' : './checked.svg'" alt="Кнопка добавление" />
    </div>
  </div>
</template>
