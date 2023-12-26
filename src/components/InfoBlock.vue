<script setup>
defineProps({
  imageUrl: String,
  title: String,
  description: String
})
</script>

<template>
  <div>
    <img width="100" height="100" class="flex text-center mx-auto" :src="imageUrl" :alt="title" />
    <h2 class="mb-5 font-bold text-xl text-center">{{ title }}</h2>
    <p class="px-5 text-l text-center w-72 text-gray-400">{{ description }}</p>
  </div>
</template>
