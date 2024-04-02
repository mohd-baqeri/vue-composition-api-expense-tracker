<template>
  <Header />

  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { computed, onMounted, ref } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, trans) => {
    return acc + trans.amount;
  }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value
    .filter((trans) => trans.amount > 0)
    .reduce((acc, trans) => {
      return acc + trans.amount;
    }, 0)
    .toFixed(2);
});

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((trans) => trans.amount < 0)
    .reduce((acc, trans) => {
      return acc + trans.amount;
    }, 0)
    .toFixed(2);
});

// Add Transaction
const handleTransactionSubmitted = (transData) => {
  transactions.value.push({
    id: generateUniqueID(),
    text: transData.text,
    amount: transData.amount,
  });

  saveTransactionsToLocalStorage();

  toast.success("Transaction added");
};

// Generate unique ID
const generateUniqueID = () => {
  return Math.floor(Math.random() * 1000000);
};

// Delete Transaction
const handleTransactionDeleted = (transId) => {
  transactions.value = transactions.value.filter(
    (trans) => trans.id !== transId
  );

  saveTransactionsToLocalStorage();

  toast.success("Transaction deleted");
};

// saveToLocalStorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>

<style scoped></style>
