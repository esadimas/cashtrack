<template>
  <h3>Add new transaction</h3>

  <form id="" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Title</label>
      <input
        type="text"
        id="title"
        placeholder="Enter text..."
        v-model="title"
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const title = ref("");
const amount = ref("");

const emit = defineEmits(["transactionSubmitted"]);

const toast = useToast();

const onSubmit = () => {
  if (!title.value || !amount.value) {
    toast.error("Both field must be filled");
    return;
  }

  const transactionData = {
    title: title.value,
    amount: parseFloat(amount.value),
  };

  emit("transactionSubmitted", transactionData);

  title.value = "";
  amount.value = "";
};
</script>
