<template>
  <div class="flex items-center border border-slate-200 p-4 rounded-xl">
    <img :src="img" class="mr-4 w-16 h-16" alt="Sneaker" />

    <div class="flex flex-col w-full">
      <div class="flex justify-between mt-5">
        <span class="font-bold">{{ price }} руб.</span>
        <img
          class="cursor-pointer opacity-30 hover:opacity-100 transition"
          src="/close.svg"
          alt="Close"
        />
      </div>
      <p>{{ title }}</p>
    </div>
  </div>
</template>
<script setup>
defineProps({
  title: {
    type: String,
    default: ''
  },
  price: {
    type: Number
  },
  img: {
    type: String
  }
})
</script>
