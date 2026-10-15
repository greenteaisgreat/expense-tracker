<script setup lang="ts">
import { ref, computed } from "vue";
import type { Transaction, Total, Income, Expense } from "./types.ts";

import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

const transactions = ref<Transaction[]>([
  { id: 1, text: "Candy", amount: 2.35 },
  { id: 2, text: "Boat", amount: -12.55 },
  { id: 3, text: "Gum", amount: 56.35 },
  { id: 4, text: "Weezel", amount: -20.72 },
]);

const total = computed<Total>(() => {
  return transactions.value.reduce(
    (acc, transaction) => transaction.amount + acc,
    0,
  );
});

const income = computed<Income>(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
});

const expense = computed<Income>(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
});
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total />
    <IncomeExpenses :income :expense />
    <TransactionList :transactions />
    <AddTransaction />
  </div>
</template>

<style scoped></style>
