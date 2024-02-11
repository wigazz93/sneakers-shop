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
    />
  </div>
</template>

<script setup>
import Card from '@/components/Card.vue'
const emits = defineEmits(['addToFavorite'])
defineProps({
  items: {
    type: Array,
    required: true
  }
})
</script>

<style lang="scss" scoped></style>
