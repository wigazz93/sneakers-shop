<template>
  <div>
    <div class="flex mb-8 items-center justify-between">
      <h2 class="text-3xl font-bold">Все кроссовки</h2>
      <div class="flex gap-4">
        <select
          @change="onChangeSelect"
          class="py-2 px-3 border rounded-md outline-none"
          name=""
          id=""
        >
          <option value="price">По цене(сначала дешевые)</option>
          <option value="-price">По (сначала дорогие)</option>
          <option value="name">По названию</option>
        </select>
        <div class="relative">
          <img class="absolute left-4 top-3" src="/search.svg" alt="search" />
          <input
            @input="onChangeSearchInput"
            class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            placeholder="Поиск..."
            type="text"
          />
        </div>
      </div>
    </div>
    <div class="mt-10">
      <CardList :items="items" @add-to-cart="clickAddPlus" @addToFavorite="addToFavorite" />
    </div>
  </div>
</template>

<script setup>
import CardList from '@/components/CardList.vue'
import axios from 'axios'
import { onMounted, ref, watch, reactive, inject } from 'vue'

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})
const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}
const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value
}

const clickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://df79a2a27ce0982b.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favItem = favorites.find((favor) => favor.item_id === item.id)
      if (!favItem) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favItem.id,
      }
    })
  } catch (error) {
    console.log(error.stack)
  }
}

const fetchItems = async () => {
  try {
    const params = { sortBy: filters.sortBy }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://df79a2a27ce0982b.mokky.dev/items', {
      params,
    })
    items.value = data.map((obj) => ({
      ...obj,
      isAdded: false,
      isFavorite: false,
    }))
  } catch (error) {
    console.log(error.stack)
  }
}
const addToFavorite = async (item) => {
  try {
    const obj = {
      item_id: item.id,
    }
    if (!item.isFavorite) {
      item.isFavorite = true
      const { data } = await axios.post('https://df79a2a27ce0982b.mokky.dev/favorites', obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://df79a2a27ce0982b.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (error) {
    console.log(error.satck)
  }
}

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }))
})

watch(filters, fetchItems)

onMounted(async () => {
  cart.value = JSON.parse(localStorage.getItem('cart')) || []
  await fetchItems()
  await fetchFavorites()
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
  }))
})
</script>

<style lang="scss" scoped></style>
