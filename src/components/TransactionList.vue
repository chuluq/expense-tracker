<script setup lang="ts">
interface Transaction {
  id: number
  text: string
  amount: number
}

const transactions: Transaction[] = [
  { id: 1, text: 'Flower', amount: -19.99 },
  { id: 2, text: 'Salary', amount: 299.97 },
]
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button class="delete-btn">x</button>
    </li>
  </ul>
</template>
