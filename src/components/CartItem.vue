<script setup>
defineProps({
  title: String,
  imageUrl: String,
  price: Number,
  isAdded: Boolean,
  onClickRemove: Function
})
</script>

<template>
  <div class="flex items-center border solide rounded-2xl relative p-4 m-3 gap-5 cursor-pointer">
    <img :src="imageUrl" :alt="title" class="w-24" />
    <div>
      <p class="text-l font">{{ title }}</p>
      <span class="font-semibold">{{ price }} руб.</span>
    </div>
    <img
      src="/close.svg"
      alt="Кнопка удаления"
      class="opacity-60 hover:opacity-90"
      @click="onClickRemove"
    />
  </div>
</template>
