<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text"/>
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input type="text" id="amount" placeholder="Enter amount..." v-model="amount" />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup lang="ts">
import type { UserTransaction } from '@/types';
import { ref } from 'vue';
import {useToast} from 'vue-toastification';
const toast = useToast();

const text  = ref('');
const amount = ref('');

const emit = defineEmits(['transactionSubmitted']);

const onSubmit = () => {

  if(!text.value || !amount.value) {
    toast.error('text and amount are compulsory fields.');
    return;
  }

  const transactionData: UserTransaction = {
    id: Math.floor(Math.random() * 100000000),
    text: text.value,
    amount: parseFloat(amount.value)
  }

  emit('transactionSubmitted', transactionData);

  text.value = '';
  amount.value = '';
}
</script>
