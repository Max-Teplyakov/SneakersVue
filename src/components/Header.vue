<script setup>
defineProps({
  totalPrice: Number
})

const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-slate-300 px-8">
    <div class="flex gap-4">
      <img src="/logo.png" alt="Логотип" class="w-10" />
      <div>
        <h2 class="text-xl font-bold">VUE SNEAKERS</h2>
        <p class="text-slate-500">Магазин лучших кросовок</p>
      </div>
    </div>
    <ul class="flex items-center gap-3">
      <li
        class="flex gap-3 cursor-pointer text-slate-500 hover:text-black"
        @click="() => emit('openDrawer')"
      >
        <img src="/cart.svg" alt="Корзина" />
        <b>{{ totalPrice }}</b>
      </li>
      <li class="flex gap-3 cursor-pointer text-slate-500 hover:text-black">
        <img src="/heart.svg" alt="Сердце" />
        <span>Закладки</span>
      </li>
      <li class="flex gap-3 cursor-pointer text-slate-500 hover:text-black">
        <img src="/profile.svg" alt="Профиль" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>
