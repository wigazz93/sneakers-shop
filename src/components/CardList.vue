<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item of items"
      :key="item.id"
      :price="item.price"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :isFavorite="item.isFavorite"
      :clickFavor="() => emits('addToFavorite', item)"
      :clickAdd="() => emits('addToCart', item)"
      :isAdded="item.isAdded"
    />
  </div>
</template>

<script setup>
import Card from '@/components/Card.vue'
const emits = defineEmits(['addToFavorite', 'addToCart'])
defineProps({
  items: {
    type: Array,
    required: true
  }
})
</script>
