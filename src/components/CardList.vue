<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item of items"
      :key="item.id"
      :price="item.price"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :isFavorite="item.isFavorite"
      :clickFavor="isFavorites ? null : () => emits('addToFavorite', item)"
      :clickAdd="isFavorites ? null : () => emits('addToCart', item)"
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
    required: true,
  },
  isFavorites: Boolean,
})
</script>
