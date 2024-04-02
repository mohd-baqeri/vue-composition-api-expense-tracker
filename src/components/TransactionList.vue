<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="trans in transactions"
      :key="trans.id"
      :class="trans.amount < 0 ? 'minus' : 'plus'"
    >
      {{ trans.text }} <span>${{ trans.amount }}</span>
      <button @click="deleteTransaction(trans.id)" class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
// import { defineProps } from "vue";

const emit = defineEmits(["transactionDeleted"]);

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

// deleteTransaction
const deleteTransaction = (transId) => {
  emit("transactionDeleted", transId);
};
</script>
