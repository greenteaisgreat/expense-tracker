<script setup lang="ts">
import { ref } from "vue";
import type { Transaction } from "@/types";

const props = defineProps<{
  transactions: Transaction[];
}>();

const emit = defineEmits<{
  deleteTransaction: [Transaction["id"]];
}>();
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <h4 v-if="transactions.length === 0" class="no-transactions">
      No Transactions to Display
    </h4>
    <li
      v-else
      v-for="transaction in props.transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text
      }}<span
        >{{ transaction.amount
        }}<button
          @click="emit('deleteTransaction', transaction.id)"
          class="delete-btn"
        >
          x
        </button></span
      >
    </li>
  </ul>
</template>

<style scoped>
.no-transactions {
  text-align: center;
  text-transform: none;
}
</style>
