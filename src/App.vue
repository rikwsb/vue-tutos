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
        {id: 1, text: 'Flower', amount: -19.99},
        {id: 2, text: 'Salary', amount: 299.90},
        {id: 3, text: 'Book', amount: -17.45},
        {id: 4, text: 'Camera', amount: -103.89}
  ])


  //Get total
  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2)
  })

  //Get income
  const income = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2)
  })

  //Get expenses
  const expenses = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2)
  })

  //Add transaction
  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    })

    console.log(generateUniqueId());
    
    toast.success('Transaction added');

  }

  //Generate id
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 10000000)
  }

</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>