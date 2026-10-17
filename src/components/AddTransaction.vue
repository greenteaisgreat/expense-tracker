<script setup lang="ts">
import { ref } from "vue";
import type { Transaction } from "@/types";
import { useToast } from "vue-toastification";

const transactionText = ref<Transaction["text"]>("");
const transactionAmount = ref<Transaction["amount"]>();
const toast = useToast();

const emit = defineEmits<{
  addTransaction: [
    transactionText: Transaction["text"],
    transactionAmount: Transaction["amount"],
  ];
}>();

function formSubmitted() {
  if (!transactionText.value || !transactionAmount.value) {
    toast.error("Please provide a value for the amount and transaction");
    return;
  } else if (transactionText.value.trim() && transactionAmount.value) {
    emit("addTransaction", transactionText.value, transactionAmount.value);
    toast.success(
      `Transaction for ${transactionText.value} added at $${transactionAmount.value}`,
    );
    transactionText.value = "";
    transactionAmount.value = 0;
  }
}
</script>
<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="formSubmitted">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        v-model="transactionText"
        type="text"
        id="text"
        placeholder="Enter text..."
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        v-model="transactionAmount"
        type="number"
        id="amount"
        placeholder="Enter amount..."
        step="0.01"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
