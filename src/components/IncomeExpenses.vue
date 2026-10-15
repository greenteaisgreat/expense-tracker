<script setup lang="ts">
import type { Income, Expense } from "@/types";

const props = defineProps<{
  income: Income;
  expense: Expense;
}>();
</script>

<template>
  <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <p id="money-plus" class="money plus">+${{ props.income.toFixed(2) }}</p>
    </div>
    <div>
      <h4>Expense</h4>
      <p id="money-minus" class="money minus">
        -${{ props.expense.toFixed(2) }}
      </p>
    </div>
  </div>
</template>
