<script setup>
import CartList from './CartList.vue'
import DrawerHead from './DrawerHead.vue'
</script>

<template>
  <div class="h-full w-full fixed top-0 left-0 bg-black opacity-70 z-10"></div>
  <div class="h-full w-110 fixed top-0 right-0 bg-white z-20">
    <div class="flex flex-col justify-between h-full">
      <div>
        <DrawerHead />
        <CartList />
      </div>
      <div class="flex flex-col justify-center p-4 gap-4">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>21 498руб.</b>
        </div>
        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>1 498руб.</b>
        </div>
        <button
          class="transition mb-8 h-14 rounded-2xl bg-lime-600 text-xl font-semibold disabled:bg-slate-300 text-white hover:bg-lime-700 active:bg-lime-800"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
