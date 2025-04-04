<template>
<div>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDelete="handleTransactionDelete" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</div>
</template>

<script setup>
import Header from './components/Header.vue' ;
import Balance from './components/Balance.vue' ;
import IncomeExpenses from './components/IncomeExpenses.vue' ;
import TransactionList from './components/TransactionList.vue' ;
import AddTransaction from './components/AddTransaction.vue' ;

import { useToast } from 'vue-toastification' ;

import { ref,computed, onMounted } from 'vue' ;

const toast = useToast() ;

const transactions = ref([]) ;

onMounted(() => {
  const saveTransactions = JSON.parse(localStorage.getItem('transactions')) ;

  if (saveTransactions) {
    transactions.value = saveTransactions ;
  }
  
}) ;

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount ;
  }, 0) ;
}) ;

const income = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount > 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount ;
  }, 0).toFixed(2) ;
}) ;

const expenses = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount < 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount ;
  }, 0).toFixed(2) ;
}) ;

const handleTransactionSubmitted = (transactionsData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionsData.text,
    amount: transactionsData.amount
  }) ;

  saveTransactionsToLocalStorage() ;

  toast.success('Transastion added') ;

} ;

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000) ;
} ;

const handleTransactionDelete = (id) => {
  transactions.value = transactions.value.filter((transaction) => 
  transaction.id !== id) ;

  saveTransactionsToLocalStorage() ;

  toast.success('Transaction deleted') ;
} ;

//Save to localstorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transaction', JSON.stringify(transactions.value)) ;
} ;
</script>