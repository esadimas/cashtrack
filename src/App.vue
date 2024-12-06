<template>
  <HeaderComponent />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="income" :expenses="expenses" />
  </div>
</template>

<script setup>
// import component - component
import HeaderComponent from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";

import { ref, computed } from "vue";

const transactions = ref([
  { text: "salary", amount: 10000 },
  { text: "mac mini m4 2024", amount: -599 },
]);

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get Income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
});

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
});
</script>
