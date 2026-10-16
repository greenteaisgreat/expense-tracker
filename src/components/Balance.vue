<script setup lang="ts">
import type { Total } from "@/types";

const props = defineProps<{
  total: Total;
}>();
</script>

<template>
  <h4>Your Balance</h4>
  <h1 class="balance">${{ props.total.toFixed(2) }}</h1>
</template>
