<script setup lang="ts">
import  UserBalance from './components/UserBalance.vue'
import AppHeader from './components/AppHeader.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import AppFooter from './components/AppFooter.vue'

import type { UserTransaction } from './types'

import { ref, computed, onMounted } from 'vue';

import {useToast} from 'vue-toastification';
const toast = useToast();


//user transactions
const transactions = ref<UserTransaction[]>([])

onMounted(() => {
  toast.success('Welcome to Vue 3 Expense Tracker App.');

  const savedTransactions = JSON.parse(localStorage.getItem('transactions') || '[]') as UserTransaction[];

  if(savedTransactions.length > 0){
    transactions.value = savedTransactions;
  }
});


//total account balance
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => { 
    return acc + transaction.amount; 
  }, 0);
})

//get income
const userIncomes = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount > 0)
  .reduce((acc, transaction) => { 
    return acc + transaction.amount; 
  }, 0).toFixed(2);
})

//get expenses
const userExpenses = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount < 0)
  .reduce((acc, transaction) => { 
    return acc + transaction.amount; 
  }, 0).toFixed(2);
})


//add transaction
const handleTransactionSubmitted = (transactionData: UserTransaction) =>{
  transactions.value.push(transactionData);
  saveTransactionToLocalStorage();
  toast.success('Transaction added successfully.');
};


//deleteTransaction
const handleTransactionDeleted = (id: Number) => {
   transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  saveTransactionToLocalStorage();
  toast.success('Transaction deleted successfully.');
}

//save transactions to local storage
const saveTransactionToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}
</script>


<template>
  <AppHeader />
  <div class="container">
    <UserBalance :total="total"/>
    <IncomeExpenses :incomes="+userIncomes" :expenses="+userExpenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    <AppFooter />
  </div>
</template>

<style scoped></style>
./types/types