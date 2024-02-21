<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Header from '@/components/Header.vue'
import Home from '@/pages/Home.vue'
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

const isCreateOrder = ref(false)
const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const vatPrice = computed(() => (totalPrice.value * 0.05).toFixed(1))

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
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
      <RouterView />
    </div>
  </div>
</template>

<style lang="scss"></style>
