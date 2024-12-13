<script setup lang="ts">
import { computed, onMounted, ref, type Ref } from "vue";
import { useToast } from "vue-toastification";
import AddTransaction from "./components/AddTransaction.vue";
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";

export interface Transaction {
  id: number;
  text: string;
  amount: number;
}

const toast = useToast();

const transactions: Ref<Transaction[]> = ref([]);

onMounted(() => {
  const savedTransactions = localStorage.getItem("transactions");

  if (typeof savedTransactions === "string") {
    transactions.value = JSON.parse(savedTransactions);
  }
});

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

const generateUniqueId = () => {
  return Date.now() + Math.random();
};

const handleTransactionSubmitted = (transactionData: { text: string; amount: number }) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  saveTransactionsToLocalStorage();

  toast.success("Transaction Added Successfully");
};

const handleDeleteTransaction = (id: number) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

  saveTransactionsToLocalStorage();

  toast.success("Transaction deleted");
};

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transaction-deleted="handleDeleteTransaction" />
    <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
  </div>
</template>
