<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { card, removeFromCart } = inject('cart')

const emit = defineEmits(['addToCart'])
</script>

<template>
  <CartItem
    v-for="item in card"
    :key="item.id"
    :title="item.title"
    :imageUrl="item.imageUrl"
    :price="item.price"
    :isAdded="item.isAdded"
    :onClickRemove="() => removeFromCart(item)"
  />
</template>
