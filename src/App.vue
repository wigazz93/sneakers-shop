<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import Drawer from '@/components/Drawer.vue'
import { computed, provide, onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'

const stateDrawer = ref(false)
const closeDrawer = () => {
  stateDrawer.value = false
}
const openDrawer = () => {
  stateDrawer.value = true
}

const cart = ref([])
const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})
const isCreateOrder = ref(false)
const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const vatPrice = computed(() => (totalPrice.value * 0.05).toFixed(1))

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const clickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}

const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://df79a2a27ce0982b.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favItem = favorites.find((favor) => favor.parentId === item.id)
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
const addToFavorite = async (item) => {
  try {
    const obj = {
      parentId: item.id,
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
const createOrder = async () => {
  try {
    isCreateOrder.value = true
    const { data } = await axios.post('https://df79a2a27ce0982b.mokky.dev/orders', {
      items: cart.value,
      totalPrice: totalPrice.value,
    })
    cart.value = []
    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreateOrder.value = false
  }
}

onMounted(async () => {
  cart.value = JSON.parse(localStorage.getItem('cart')) || []
  await fetchItems()
  await fetchFavorites()
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
  }))
})

watch(filters, fetchItems)

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }))
})
watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true },
)

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart,
  totalPrice,
  vatPrice,
})
</script>

<template>
  <Drawer
    v-if="stateDrawer"
    :isCreateOrder="isCreateOrder"
    :totalPrice="totalPrice"
    :vatPrice="vatPrice"
    @createOrder="createOrder"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header :totalPrice="totalPrice" @open-drawer="openDrawer" />
    <div class="p-10">
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
      <CardList :items="items" @add-to-cart="clickAddPlus" @addToFavorite="addToFavorite" />
    </div>
  </div>

  <RouterView />
</template>

<style lang="scss"></style>
