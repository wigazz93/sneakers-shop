<template>
  <div
    class="bg-white flex flex-col justify-between mt-2 relative border border-slate-100 rounded-xl p-8 cursor-pointer transition hover:shadow-xl hover:transform hover:-translate-y-2"
  >
    <div>
      <img
        v-if="clickFavor"
        @click="clickFavor"
        :src="isFavorite ? '/like-2.svg' : '/like-1.svg'"
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
      <img
        v-if="clickAdd"
        @click="clickAdd"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="Plus"
      />
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  key: {
    type: Number,
  },
  imageUrl: {
    type: String,
  },
  title: {
    type: String,
    required: true,
  },
  price: {
    type: Number,
    required: true,
  },
  isFavorite: {
    type: Boolean,
    default: false,
  },
  isAdded: {
    type: Boolean,
    default: false,
  },
  clickFavor: {
    type: Function,
  },
  clickAdd: {
    type: Function,
  },
})
</script>

<style lang="scss" scoped></style>
