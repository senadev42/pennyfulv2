<script setup>
import { ref, computed, onMounted } from 'vue';



// Components
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue'
import mockdata from "./mockdata.json"
import Settings from './components/Settings.vue'

// on mount
onMounted(() => {

  //sanity
  if (!localStorage.getItem('transactions')) {
    localStorage.setItem('transactions', JSON.stringify([]));
  }

  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (datasource.value == "localstorage") {
    transactions.value = savedTransactions
  } else {
    transactions.value = mockdata.transactions;
  }

});

//data source logic
let transactions = ref([]);

let datasource = ref('mock');

const changedatasource = () => {
  console.log(datasource.value);

  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (datasource.value == 'mock') {
    console.log("changing to localstorage")
    datasource.value = 'localstorage';
    transactions.value = savedTransactions || [];
    return;
  }
  if (datasource.value == 'localstorage') {
    console.log("changing to mock")
    datasource.value = 'mock';
    transactions.value = mockdata.transactions;
    return;
  }
}



//computed values
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


// handlers
const handleTransactionSubmitted = (transactionData) => {
  console.log("inapp", transactionData);

  transactions.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: transactionData.text,
    amount: transactionData.amount,
    date: transactionData.date
  });

  if (datasource.value == 'localstorage') saveTransactionsToLocalStorage();
};

const handleTransactionDeleted = (id) => {
  console.log("deleting", id);

  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );

  if (datasource.value == 'localstorage') saveTransactionsToLocalStorage();
};

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};


//export and import
const exportdata = (livetransactions) => {
  const transactions = livetransactions;

  console.log(livetransactions);

  // Create CSV content
  const csvContent = "data:text/csv;charset=utf-8," + transactions.map(transaction => Object.values(transaction).join(',')).join('\n');

  // Create a data URI and create an anchor element for download
  const encodedUri = encodeURI(csvContent);
  const link = document.createElement("a");
  link.setAttribute("href", encodedUri);
  link.setAttribute("download", "transactions.csv");

  // Trigger a click on the anchor element to start the download
  document.body.appendChild(link); // Required for Firefox
  link.click();
  document.body.removeChild(link); // Clean up

  console.log("CSV exported");
}



</script>


<template>
  <div class="bg-gray-100 min-h-screen pb-8 flex flex-col items-center">

    <div class="grid grid-cols-1 w-[90%] md:w-[33rem] mx-auto gap-y-2">

      <div class="mt-10">
        <Balance :total="total" />
      </div>


      <IncomeExpenses :income="+income" :expenses="+expenses" />

      <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />

      <!-- switch data source -->
      <Settings :exportdata="exportdata" :transactions="transactions" :changedatasource="changedatasource"
        v-model:datasource="datasource" />

      <div class="mt-2">
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
      </div>

    </div>
  </div>
</template>



<style></style>