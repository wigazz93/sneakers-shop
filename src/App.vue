<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Hello from '@/components/Hello.vue'
</script>

<template>
  <header class="mb-40 ml-50">
    <Hello />
  </header>

  <RouterView />
</template>

<style></style>
