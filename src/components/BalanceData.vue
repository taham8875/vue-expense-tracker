<script setup lang="ts">
defineProps({
  balance: Number,
  income: Number,
  expense: Number,
});
</script>

<template>
  <div class="py-6">
    <h3>Total Balance</h3>
    <h2 class="text-3xl font-semibold">${{ balance }}</h2>
  </div>
  <div class="flex align-center justify-around px-2 py-6 border border-gray-200 rounded">
    <div>
      <h4 class="text-green-500">Income</h4>
      <p class="text-green-500">${{ income }}</p>
    </div>
    <div>
      <h4 class="text-red-500">Expense</h4>
      <p class="text-red-500">${{ expense }}</p>
    </div>
  </div>
</template>