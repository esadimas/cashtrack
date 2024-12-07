<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li v-if="transactions.length === 0" class="no-transaction">
      No transaction
    </li>
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.title }} <span>${{ transaction.amount }}</span>
      <button @click="deleteTransaction(transaction.id)" class="delete-btn">
        x
      </button>
    </li>
  </ul>
</template>

<script setup>
import { defineProps } from "vue";

const emit = defineEmits(["transactionDelete"]);

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const deleteTransaction = (id) => {
  emit("transactionDelete", id);
};
</script>
