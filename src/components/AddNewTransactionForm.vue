<script setup lang="ts">
import Button from './Button.vue';
import type { TransactionType } from '../../index.d';
import { ref, watchEffect } from 'vue';

const transactionDetails = ref({
  name: '',
  amount: '',
})

const emit = defineEmits(['create-new-transaction']);

const errorMessage = ref<string>('');

function emitAddNewTransaction() {
  if (transactionDetails.value.name === '' || transactionDetails.value.amount === '') {
    errorMessage.value = 'Please fill out all fields';
    return;
  }
  // check if amount can be converted to a number
  if (isNaN(Number(transactionDetails.value.amount))) {
    errorMessage.value = 'Please enter a valid number';
    return;
  }

  emit('create-new-transaction', {
    name: transactionDetails.value.name,
    amount: Number(transactionDetails.value.amount),
  });

  transactionDetails.value = {
    name: '',
    amount: '',
  };
}



watchEffect(() => {
  if (transactionDetails.value.name !== '' || transactionDetails.value.amount !== '') {
    errorMessage.value = '';
  }
});
</script>

<template>
  <h2 class="text-xl font-semibold">Add New Transaction</h2>
  <form @submit.prevent="emitAddNewTransaction" class="flex flex-col gap-4 mt-3">

    <label class="flex flex-col">Transaction Name
      <input class="border border-gray-200 p-2 rounded" type="text" id="transaction-name"
        v-model="transactionDetails.name" />
    </label>

    <label class="flex flex-col relative">Amount
      <input class="border border-gray-200 p-2 rounded" type="text" id="transaction-day"
        v-model="transactionDetails.amount" />
    </label>


    <p class="text-red-500">{{ errorMessage }}</p>

    <Button type="submit" class="w-full">Create Transaction</Button>
  </form>
</template>