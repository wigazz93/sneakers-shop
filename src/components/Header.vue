<template>
  <header class="flex justify-between border-b border-slate-300 px-8 py-3">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="logo" class="w-10" />
        <h2 class="text-xl font-bold uppercase">Vue sneakers</h2>
        <p class="text-slate-400">Магазин кроссовок</p>
      </div>
    </router-link>
    <ul class="flex items-center gap-10">
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer"
      >
        <img src="/cart.svg" alt="Cart" />
        <span>{{ totalPrice }} руб</span>
      </li>
      <router-link to="/favorites">
        <li>
          <img src="/heart.svg" alt="Heart" />
          <b>Закладки</b>
        </li>
      </router-link>
      <li>
        <img src="/profile.svg" alt="Profile" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>

<script setup>
const props = defineProps({
  totalPrice: Number,
})
console.log(props.totalPrice)
const emit = defineEmits(['openDrawer'])
</script>

<style lang="scss"></style>
