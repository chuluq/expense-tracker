<script setup lang="ts">
import type { Transaction } from '@/App.vue'
import { defineProps } from 'vue'

const props = defineProps<{ transactions: Transaction[] }>()
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in props.transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button class="delete-btn">x</button>
    </li>
  </ul>
</template>
