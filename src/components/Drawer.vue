<template>
  <div>
    <div class="fixed z-10 top-0 h-full w-full bg-black opacity-70"></div>
    <div
      class="flex flex-col justify-between fixed h-full z-10 top-0 h-full right-0 w-96 bg-white px-10 py-7"
    >
      <DrawerHead />
      <CartItemList />
    </div>
  </div>
</template>

<script setup>
import CartItemList from '@/components/CartItemList.vue'
import DrawerHead from '@/components/DrawerHead.vue'
</script>

<style lang="scss"></style>
