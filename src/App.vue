<script setup lang="ts">
import Header from './components/Header.vue';
import BalanceData from './components/BalanceData.vue';
import TransactionsHistory from './components/TransactionsHistory.vue';
import AddNewTransactionForm from './components/AddNewTransactionForm.vue';

import { ref, onMounted, computed } from 'vue';
import type { TransactionType } from '../index.d';

const transactions = ref<TransactionType[]>([]);

onMounted(async () => {
  const response = await fetch('api/transactions');
  transactions.value = await response.json();
  console.log(transactions.value);

  transactions.value = transactions.value.map((transaction) => {
    return {
      ...transaction,
      amount: Number(transaction.amount),
    };
  });
});

const balance = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return transaction.amount > 0 ? acc + transaction.amount : acc;
  }, 0);
});

const expense = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return transaction.amount < 0 ? acc + transaction.amount : acc;
  }, 0);
});

const createNewTransaction = async (transaction: TransactionType) => {
  const response = await fetch('api/transactions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(transaction),
  });

  const newTransaction = await response.json();
  transactions.value = [...transactions.value, newTransaction];
};

const deleteTransaction = async (id: string) => {
  console.log(id);
  await fetch(`api/transactions/${id}`, {
    method: 'DELETE',
  });

  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
};
</script>

<template>
  <Header />
  <BalanceData :balance="balance" :expense="expense" :income="income" />
  <TransactionsHistory :transactions="transactions" @delete-transaction="deleteTransaction" />
  <AddNewTransactionForm @create-new-transaction="createNewTransaction" />
</template>

<style>
#app {
  @apply max-w-xl mx-auto my-3 px-8 py-5 border border-gray-200 rounded;
}
</style>