<template>
  <div>
    <Header />
    <div class="container">
      <Balance :total="total" />
      <income-expenses :income="income" :expenses="expenses" />
      <transaction-list :transactions="transactions" />
      <add-transaction />
    </div>
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed } from "vue";

const transactions = ref([
  { id: 1, text: "Salary", amount: 7500000 },
  { id: 1, text: "Plant", amount: -4490.85 },
  { id: 1, text: "Book", amount: 91542.1 },
  { id: 1, text: "Liquor", amount: -87518 },
]);
const total = computed(() => {
  return transactions.value.reduce((acc, trans) => {
    return acc + trans.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((e) => e.amount > 0)
    .reduce((acc, trans) => {
      return acc + trans.amount;
    }, 0);
});

const expenses = computed(() => {
  return transactions.value
    .filter((e) => e.amount < 0)
    .reduce((acc, trans) => {
      return acc + trans.amount;
    }, 0);
});
</script>

<style scoped></style>
