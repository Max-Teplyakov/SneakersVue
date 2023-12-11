<script setup>
defineProps({
  title: String,
  imageUrl: String,
  price: Number,
  isFavorited: Boolean,
  isAdded: Boolean
})
</script>

<template>
  <div
    class="border solide rounded-2xl relative p-8 m-3 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
  >
    <img :src="!isFavorited ? '/like-1.svg' : '/like-2.svg'" alt="Лайк 1" class="absolute" />
    <img src="/sneakers/sneakers-7.jpg" alt="Кросовок" />
    <p class="text-l font-bold">Мужские Кроссовки Nike Blazer Mid Suede</p>
    <div class="flex justify-between">
      <p>Цена: <span class="font-bold">12 999 руб.</span></p>
      <img :src="!isAdded ? '/plus.svg' : './checked.svg'" alt="Кнопка добавление" />
    </div>
  </div>
</template>
