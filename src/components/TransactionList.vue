<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="(transaction, i) in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button class="delete-btn" @click="DeleteFun(i)">x</button>
    </li>
  </ul>
</template>

<script setup>
const props = defineProps({
  transactions: Array,
});

const emit = defineEmits(["deleteTransaction"]);

function DeleteFun(i) {
  emit("deleteTransaction", i);
}
</script>
