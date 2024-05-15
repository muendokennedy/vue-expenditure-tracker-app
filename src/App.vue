<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { useToast } from 'vue-toastification'

import { ref, computed } from 'vue'

const toast = useToast()

const transactions = ref([
        { id: 1 , text: 'Flower', amount: -19.9},
        { id: 2 , text: 'Salary', amount: 299.7},
        { id: 3 , text: 'Book', amount: -10},
        { id: 4 , text: 'Camera', amount: 150}
      ])
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
  toast.success('Transaction added successfully')
}
// Generate the unique id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}
</script>
<template>
  <div class="container">
    <Header/>
    <Balance :total="+total"/>
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>
<style scoped>

</style>