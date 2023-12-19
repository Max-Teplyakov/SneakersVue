<script setup>
import { inject } from 'vue'

const { closeDrawer } = inject('cart')
</script>

<template>
  <div class="flex gap-4 p-4 cursor-pointer transition hover:-translate-x-1" @click="closeDrawer">
    <img src="/arrow-next.svg" alt="Стрелка" class="bg-black rotate-180" />
    <p class="font-bold text-2xl">Корзина</p>
  </div>
</template>
