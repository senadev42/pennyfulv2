<script setup>
import { ref, computed } from 'vue';

// Components
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue'
import mockdata from "./mockdata.json"

// State
let transactions = ref(mockdata.transactions);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});


const handleTransactionSubmitted = (transactionData) => {
  console.log("inapp", transactionData);

  const currentDate = new Date();
  const formattedDate = `${currentDate.getFullYear()}-${String(currentDate.getMonth() + 1).padStart(2, '0')}-${String(currentDate.getDate()).padStart(2, '0')}`;


  transactions.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: transactionData.text,
    amount: transactionData.amount,
    date: formattedDate
  });
};

const handleTransactionDeleted = (id) => {
  console.log("deleting", id);

  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
};

</script>


<template>
  <div class="bg-gray-100 min-h-screen flex flex-col items-center pb-8">

    <div class="md:p-2 py-2 flex flex-col gap-y-4 mx-2">
      <!-- <Header /> -->
      <Balance :total="total" />
      <IncomeExpenses :income="+income" :expenses="+expenses" />
      <div class="mt-2">
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
      </div>

      <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
  </div>
</template>



<style></style>