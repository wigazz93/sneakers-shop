<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Header from '@/components/Header.vue'
</script>

<template>
  <div class="h-screen bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />
  </div>

  <RouterView />
</template>

<style lang="scss"></style>
