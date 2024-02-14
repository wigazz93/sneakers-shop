<template>
  <div class="flex flex-col gap-4">
    <CartItem
      v-for="item of cart"
      :key="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      @clickRemove="removeFromCart(item)"
    />
  </div>
</template>

<script setup>
import CartItem from '@/components/CartItem.vue'
import { inject } from 'vue'

const { cart, removeFromCart } = inject('cart')
</script>
