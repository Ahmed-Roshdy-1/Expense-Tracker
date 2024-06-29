<template>
  <Header />
  <div class="container">
    <Balance :balance="balance" />
    <IncomeExpenses :income="income" :expense="expense" />
    <TransactionList
      :transactions="transactions"
      @delete-transaction="deleteTransaction"
    />
    <AddTransaction @addData="addData" />
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

const balance = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    // console.log(acc, transaction.amount);
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
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

function addData(value) {
  transactions.value.push(value);
  toast.success("Transaction added successfully");
  saveTransactionToLocalstorage();
}

function deleteTransaction(value) {
  transactions.value.splice(value, 1);
  toast.success("Transaction deleted successfully");
  saveTransactionToLocalstorage();
}

function saveTransactionToLocalstorage() {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
}

onMounted(() => {
  let data = JSON.parse(localStorage.getItem("transactions"));
  console.log(data);

  if (data) {
    transactions.value = data;
  }
});
</script>

<style scoped></style>
