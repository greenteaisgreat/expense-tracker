<script setup lang="ts">
import { ref, computed } from "vue";
import type { Transaction, Total, Income, Expense } from "./types.ts";

import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

const id = ref(0);

const transactions = ref<Transaction[]>([
  { id: id.value++, text: "Candy", amount: 2.35 },
  { id: id.value++, text: "Boat", amount: -12.55 },
  { id: id.value++, text: "Gum", amount: 56.35 },
  { id: id.value++, text: "Weezel", amount: -20.72 },
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
    id: id.value++,
    text: transactionText,
    amount: transactionAmount,
  });
}

function deleteTransaction(id: number) {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id,
  );
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
