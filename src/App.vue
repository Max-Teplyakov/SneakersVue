<script setup>
import { ref, onMounted, watch, reactive, provide } from 'vue'
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
import axios from 'axios'

const arrCards = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://116d2d4d9a1012c8.mokky.dev/favorites`)
    arrCards.value = arrCards.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)
      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoritedId: favorite.id
      }
    })
  } catch (e) {
    console.log(e)
  }
}

const addToFavorite = async (item) => {
  console.log(item.isFavorite)
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }
      item.isFavorite = true

      const { data } = await axios.post(`https://116d2d4d9a1012c8.mokky.dev/favorites`, obj)
      item.favoriteId = data.id
      console.log(item)
    } else {
      await axios.delete(`https://116d2d4d9a1012c8.mokky.dev/favorites/${item.favoriteId}`)
      item.isFavorite = false
      item.favoriteId = null
    }
  } catch (e) {
    console.log(e)
  }
}

const fetchitems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get(`https://116d2d4d9a1012c8.mokky.dev/card`, { params })
    arrCards.value = data.map((card) => ({
      ...card,
      isFavorite: false,
      favoritedId: null,
      isAdded: false
    }))
  } catch (e) {
    console.log(e)
  }
}

onMounted(() => {
  fetchitems()
  fetchFavorites()
})

watch(filters, fetchitems)

provide('addToFavorite', addToFavorite)

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
