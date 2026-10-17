<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
import type { Transaction, Total, Income, Expense } from "./types.ts";
import { useToast } from "vue-toastification";

import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

onMounted(() => {
  // retrieve stored transactions, if they exist
  const savedTransactions = localStorage.getItem("transactions");
  if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions);
  }
});

const transactions = ref<Transaction[]>([]);
const toast = useToast();

// calculate total amount for all transactions
const total = computed<Total>(() => {
  return transactions.value.reduce(
    (acc, transaction) => transaction.amount + acc,
    0,
  );
});

// calculate total for income
const income = computed<Income>(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
});

// calculate total for expenses
const expense = computed<Expense>(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
});

function addTransaction(
  transactionText: Transaction["text"],
  transactionAmount: Transaction["amount"],
) {
  transactions.value.push({
    id: crypto.randomUUID(),
    text: transactionText,
    amount: transactionAmount,
  });
  saveTransactionsToLocalStorage();
}

function deleteTransaction(id: string) {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id,
  );
  saveTransactionsToLocalStorage();
  toast.success(`Transaction successfully deleted`);
}

function saveTransactionsToLocalStorage() {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
}
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total />
    <IncomeExpenses :income :expense />
    <TransactionList :transactions @delete-transaction="deleteTransaction" />
    <AddTransaction @add-transaction="addTransaction" />
  </div>
</template>

<style scoped></style>
