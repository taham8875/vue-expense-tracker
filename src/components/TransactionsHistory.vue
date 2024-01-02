<script setup lang="ts">
import type { TransactionType } from 'index';
import CancelIcon from './CancelIcon.vue';

defineProps({
  transactions: Array as () => TransactionType[],
});

defineEmits(['delete-transaction']);
</script>

<template>
  <div class="mt-4">
    <h2 class="text-xl font-semibold">History</h2>
    <hr class="my-2">
    <ul>
      <div v-for="transaction in transactions" :key="transaction.id" class="flex">
        <li
          class="group flex justify-between w-full border rounded border-gray-200 border-r-4 hover:scale-105 transition-transform my-4 p-4 group"
          :class="transaction.amount > 0 ? 'border-r-green-600' : 'border-r-red-500'">
          <CancelIcon
            class="absolute -translate-x-4 -translate-y-4 scale-[60%] opacity-0 group-hover:opacity-100 text-red-600 transition-opacity cursor-pointer"
            @click="$emit('delete-transaction', transaction.id)" />
          {{ transaction.name }} <span>${{ transaction.amount }}</span>
        </li>
      </div>
    </ul>
  </div>
</template>