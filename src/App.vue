<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import Drawer from '@/components/Drawer.vue'
import { onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'

const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}

const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value
}

const fetchItems = async () => {
  const params = { sortBy: filters.sortBy }

  if (filters.searchQuery) {
    params.title = `*${filters.searchQuery}*`
  }

  try {
    const { data } = await axios.get('https://df79a2a27ce0982b.mokky.dev/items', {
      params
    })
    items.value = data
  } catch (error) {
    console.log(error.stack)
  }
}

onMounted(fetchItems)

watch(filters, fetchItems)
</script>

<template>
  <!-- <Drawer /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />

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
      <CardList :items="items" />
    </div>
  </div>

  <RouterView />
</template>

<style lang="scss"></style>
