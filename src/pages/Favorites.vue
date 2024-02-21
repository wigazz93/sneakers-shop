<template>
  <div>
    <h2 class="text-3xl font-bold">Мои закладки</h2>
    <CardList :items="favorites" isFavorites />
  </div>
</template>

<script setup>
import axios from 'axios'
import { ref, onMounted } from 'vue'
import CardList from '@/components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://df79a2a27ce0982b.mokky.dev/favorites?_relations=items',
    )

    favorites.value = data.map((obj) => obj.item)
    console.log(data)
  } catch (error) {
    console.log(error)
  }
})
</script>

<style lang="scss"></style>
