<script setup>
import { inject } from 'vue'
import Card from './Card.vue'

defineProps({
  items: Array
})
const addToFavorite = inject('addToFavorite')
</script>

<template>
  <div class="grid grid-cols-4">
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :onClickFavorite="() => addToFavorite(item)"
      :isFavorite="item.isFavorite"
    />
  </div>
</template>
