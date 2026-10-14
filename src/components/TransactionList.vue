<script setup lang="ts">
import { ref } from "vue";
import type { Transaction } from "@/types";

const props = defineProps<{
  transactions: Transaction[];
}>();
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in props.transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text
      }}<span
        >{{ transaction.amount }}<button class="delete-btn">x</button></span
      >
    </li>
  </ul>
</template>
