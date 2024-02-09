<template>
  <div class="flex flex-col gap-4">
    <CartItem img="/sneakers/sneakers-1.jpg" title="Nike air max 2015" :price="2000" />
    <CartItem img="/sneakers/sneakers-1.jpg" title="Nike air max 2015" :price="2000" />
    <CartItem img="/sneakers/sneakers-1.jpg" title="Nike air max 2015" :price="2000" />
    <CartItem img="/sneakers/sneakers-1.jpg" title="Nike air max 2015" :price="2000" />
  </div>
</template>

<script setup>
import CartItem from '@/components/CartItem.vue'
</script>

<style lang="scss"></style>
