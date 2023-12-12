<script setup>
import { ref, onMounted, watch, reactive } from 'vue'
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
import axios from 'axios'

const arrCards = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const fetchitems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get(`https://116d2d4d9a1012c8.mokky.dev/card`, { params })
    arrCards.value = data
  } catch (e) {
    console.log(e)
  }
}

onMounted(fetchitems)

watch(filters, fetchitems)

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}

const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value
}
</script>
<template>
  <!-- <Drawer /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />
    <div class="p-10 flex justify-between items-center">
      <h2 class="text-3xl font-bold">Все кросовки</h2>
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-xl">
        <option value="name">По названию</option>
        <option value="price">По цене(самые дешевые)</option>
        <option value="-price">По цене(самые дорогие)</option>
      </select>
      <div class="relative">
        <img class="absolute top-3 left-4" src="/search.svg" alt="поиск" />
        <input
          @input="onChangeSearchInput"
          type="text"
          class="border rounded-xl py-2 pl-10 pr-3 outline-none focus:border-gray-500"
          placeholder="Поиск..."
        />
      </div>
    </div>
    <CardList :items="arrCards" />
  </div>
</template>
