<script setup lang="ts">
import { defineProps } from "vue";

defineProps({
  total: {
    type: Number,
    required: true,
  },
});
</script>
<script lang="ts">
export default {
  name: "BalanceItem",
};
</script>

<template>
  <h4>Your Balance</h4>
  <h1 id="balance">${{ total }}</h1>
</template>
