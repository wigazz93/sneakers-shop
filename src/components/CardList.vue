<template>
  <div class="grid grid-cols-4 gap-5">
    <Card :price="1205" imgUrl="/sneakers/sneakers-1.jpg" title="Мужские кроссовки" />
    <Card />
    <Card />
    <Card />
    <Card />
    <Card />
  </div>
</template>

<script setup>
import Card from '@/components/Card.vue'
</script>

<style lang="scss" scoped></style>
