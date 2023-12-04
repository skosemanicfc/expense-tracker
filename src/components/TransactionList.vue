<script setup lang="ts">

import { defineProps } from 'vue';
import  type {UserTransaction } from '../types';

const props = defineProps({
  transactions:{
    type: Array as () => UserTransaction[],
    required: true
  }
});

const emit = defineEmits(['transactionDeleted']);

const deleteTransaction = (id: number) => {
  emit('transactionDeleted', id);
}

</script>
<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li class="plus" v-for="transaction in transactions" v-bind:key="transaction.id"
    :class="transaction.amount < 0 ? 'minus' : 'plus'">
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
    </li>
  </ul>
</template>
