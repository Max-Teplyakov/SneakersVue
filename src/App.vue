<script setup>
import { ref, onMounted, watch, reactive, provide, computed } from 'vue'
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
import axios from 'axios'
// Все Кросовки
const arrCards = ref([])
// Кросовки в корзине
const card = ref([])
const drawerOpen = ref(false)
const isCreatingOrder = ref(false)

const totalPrice = computed(() => card.value.reduce((acc, item) => acc + item.price, 0))
const taxPrice = computed(() => Math.round(totalPrice.value * 5) / 100)
const cartIsEmpty = computed(() => card.value.length === 0)
const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value)

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post(`https://116d2d4d9a1012c8.mokky.dev/orders`, {
      items: card.value,
      totalPrice: totalPrice.value
    })
    card.value = []
    return data
  } catch (e) {
    console.log(e)
  } finally {
    isCreatingOrder.value = false
  }
}

const addToCart = (item) => {
  card.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  card.value.splice(card.value.indexOf(item), 1)
  item.isAdded = false
}

const handleClickCart = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

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
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }
      item.isFavorite = true

      const { data } = await axios.post(`https://116d2d4d9a1012c8.mokky.dev/favorites`, obj)
      item.favoriteId = data.id
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

onMounted(async () => {
  const localCards = localStorage.getItem('card')
  card.value = localCards ? JSON.parse(localCards) : []
  await fetchitems()
  await fetchFavorites()

  arrCards.value = arrCards.value.map((item) => ({
    ...item,
    isAdded: card.value.some((cardItem) => cardItem.id === item.id)
  }))
})

watch(filters, fetchitems)
watch(card, () => {
  arrCards.value = arrCards.value.map((card) => ({
    ...card,
    isAdded: false
  }))
})

watch(
  card,
  () => {
    localStorage.setItem('card', JSON.stringify(card.value))
  },
  {
    deep: true
  }
)

provide('addToFavorite', addToFavorite)
provide('cart', {
  card,
  openDrawer,
  closeDrawer,
  addToCart,
  removeFromCart
})

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}

const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value
}
</script>
<template>
  <Drawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    :tax-price="taxPrice"
    @create-order="createOrder"
    :button-disabled="cartButtonDisabled"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header @openDrawer="openDrawer" :total-price="totalPrice" />
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
    <CardList :items="arrCards" @handle-click-cart="handleClickCart" />
  </div>
</template>
