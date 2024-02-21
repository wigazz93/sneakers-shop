<template>
  <div>
    <div @click="closeDrawer" class="fixed z-10 top-0 h-full w-full bg-black opacity-70"></div>
    <div
      class="flex flex-col justify-between fixed h-full z-10 top-0 right-0 w-96 bg-white px-10 py-7"
    >
      <DrawerHead :close="closeDrawer" />
      <div v-if="totalPrice" class="flex flex-col flex-1 justify-between">
        <div>
          <div class="flex flex-col gap-5">
            <div class="flex items-end gap-2">
              <span>Итого:</span>
              <div class="flex-1 border-b border-dashed"></div>
              <span class="font-bold">{{ totalPrice }}руб.</span>
            </div>

            <div class="flex items-end gap-2">
              <span>Налог 5%:</span>
              <div class="flex-1 border-b border-dashed"></div>
              <span class="font-bold">{{ vatPrice }} руб.</span>
            </div>
          </div>
          <div
            :class="[
              { disabled: isDisabled },
              'cursor-pointer',
              'flex',
              'justify-center',
              'items-center',
              'gap-3',
              ' w-full',
              'py-3',
              'mt-10',
              'bg-lime-500',
              'text-white',
              'rounded-xl',
              'transition',
              'active:bg-lime-700',
              'hover:bg-lime-600',
            ]"
            @click="makeOrder"
          >
            Оформить заказ
            <img src="/arrow-next.svg" alt="Arrow" />
          </div>
        </div>
      </div>
      <div v-if="!totalPrice && !isOrdered" class="flex h-full items-center">
        <InfoBlock
          imageUrl="package-icon.png"
          title="Корзина пуста"
          description="Добавьте что-нибудь в корзину"
        />
      </div>
      <div v-if="isOrdered" class="flex h-full items-center">
        <InfoBlock
          imageUrl="order-success-icon.png"
          title="Заказ оформлен"
          description="Спасибо за покупку"
        />
      </div>
      <CartItemList v-if="totalPrice" />
    </div>
  </div>
</template>

<script setup>
import CartItemList from '@/components/CartItemList.vue'
import InfoBlock from '@/components/InfoBlock.vue'
import DrawerHead from '@/components/DrawerHead.vue'
import { inject, ref } from 'vue'
const { closeDrawer } = inject('cart')

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
  isCreateOrder: Boolean,
})
const emit = defineEmits(['createOrder'])
const isDisabled = ref(true)
const isOrdered = ref(false)

if (props.totalPrice > 0 || props.isCreateOrder) {
  isDisabled.value = false
}

const makeOrder = () => {
  emit('createOrder')
  isDisabled.value = true
  isOrdered.value = true
}
</script>
<style>
.disabled {
  background-color: #686666;
  cursor: not-allowed;
  pointer-events: none;
}
.disabled:hover {
  background-color: #686666;
}
</style>
