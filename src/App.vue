<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Hello from '@/components/Hello.vue'
</script>

<template>
  <div class="h-screen bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <h1>Hello</h1>
  </div>

  <RouterView />
</template>

<style></style>
