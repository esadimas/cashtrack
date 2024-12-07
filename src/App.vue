<template>
  <HeaderComponent />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList :transactions="transactions" @transaction-delete="handleTransactionDeleted" />
    <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
// import component - component
import HeaderComponent from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

const transactions = ref([]);

const toast = useToast()

onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransaction) {
    transactions.value = savedTransaction;
  }
});

// save transaction to localstorage
const savedTransactionToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

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

// Add Transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    title: transactionData.title,
    amount: transactionData.amount,
  })

  savedTransactionToLocalStorage();

  toast.success('Transaction added');
}

const generateUniqueId = () => {
  return Math.floor(Math.random() * 100000);
}

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(transaction => transaction.id !== id);

  savedTransactionToLocalStorage();

  toast.success("Transaction deleted");
}

</script>
