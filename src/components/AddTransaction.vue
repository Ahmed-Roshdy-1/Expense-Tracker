<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input type="number" v-model="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref("");
const emit = defineEmits(["addData"]);
const toast = useToast();
function onSubmit() {
  if (!text.value && !amount.value) {
    toast.error("Both Fields must be filled");
    return;
  }
  let data = {
    id: Math.floor(Math.random() * 1000000),
    text: text.value,
    amount: parseFloat(amount.value),
  };
  emit("addData", data);
  text.value = "";
  amount.value = "";
}
</script>
