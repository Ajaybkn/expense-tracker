<template>
  <div>
    <Header />
    <div class="container">
      <Balance :total="+total" />
      <income-expenses :income="income" :expenses="+expenses" />
      <transaction-list
        :transactions="transactions"
        @transactionDeleted="handleTransactionDeleted"
      />
      <add-transaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";
import { ref, computed } from "vue";

const toast = useToast();
const transactions = ref([
  { id: 1, text: "Salary", amount: 7500000 },
  { id: 2, text: "Plant", amount: -4490.85 },
  { id: 3, text: "Book", amount: 91542.1 },
  { id: 4, text: "Liquor", amount: -87518 },
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

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  toast.success("Record saved successfully");
};

const generateUniqueId = () => {
  return Math.floor(Math.random() * 100000);
};
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((x) => x.id != id);
  toast.success("deleted");
};
</script>

<style scoped></style>
