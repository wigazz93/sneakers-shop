<template>
  <div
    class="bg-white flex flex-col justify-between mt-2 relative border border-slate-100 rounded-xl p-8 cursor-pointer transition hover:shadow-xl hover:transform hover:-translate-y-2"
  >
    <div>
      <img
        :src="isFavorite ? '/like-1.svg' : '/like-2.svg'"
        alt="Favorite"
        class="absolute top-8 left-8"
      />
    </div>
    <img :src="imageUrl" alt="Sneaker" />
    <p>{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col gap-2">
        <span class="text-slate-400">Цена:</span>
        <span class="font-bold">{{ price }}руб.</span>
      </div>
      <img :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="Plus" />
    </div>
  </div>
</template>

<script setup>
defineProps({
  imageUrl: {
    type: String
  },
  title: {
    type: String,
    required: true
  },
  price: {
    type: Number,
    required: true
  },
  isFavorite: {
    type: Boolean,
    required: false
  },
  isAdded: {
    type: Boolean,
    default: false
  }
})
</script>

<style lang="scss" scoped></style>
