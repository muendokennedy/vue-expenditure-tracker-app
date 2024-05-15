<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { useToast } from 'vue-toastification'

import { ref, computed, onMounted } from 'vue'

const toast = useToast()

const transactions = ref([])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if(savedTransactions){
    transactions.value = savedTransactions
  }
})
// compute the total expenditure
const total = computed(() => {
  return transactions.value.reduce((accumulator, transaction) => {
    return accumulator + transaction.amount
  }, 0).toFixed(2)
})
// compute the total income
const income = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount > 0).reduce((accumulator, transaction) => {
    return accumulator + transaction.amount
  }, 0).toFixed(2)
})

// compute the total expenditure
const expenses = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount < 0).reduce((accumulator, transaction) => {
    return accumulator + transaction.amount
  }, 0).toFixed(2)
})

// Add the transactons from the user
const handleTransactionSubmitted = (data) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: data.text,
    amount: data.amount
  })

  savedTransactionsToLocalStorage()

  toast.success('Transaction added successfully')
}
// Generate the unique id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}
// Delete a transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => (
    transaction.id !== id
  ))

  savedTransactionsToLocalStorage()
  
  toast.success('Transaction deleted successfully')
}

// Save to local storage
const savedTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>
<template>
  <div class="container">
    <Header/>
    <Balance :total="+total"/>
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>
<style scoped>

</style>