<script setup>

import { ref } from 'vue';

const text = ref('');
const amount = ref('');

let submitemit = defineEmits(['transactionSubmitted']);

const onSubmit = () => {
    //basic validation
    if (!text.value || !amount.value) {
        console.log('Both fields must be filled.');
        return;
    }

    const onetransaction = {
        text: text.value,
        amount: parseFloat(amount.value),
    };

    submitemit('transactionSubmitted', onetransaction);

    // clear field?
    // text.value = '';
    // amount.value = '';
};




</script>

<template>
    <div class="mt-2">
        <form id="form" @submit.prevent="onSubmit"
            class="bg-white p-4 rounded-md shadow-md text-xs md:text-sm flex flex-col items-stretch">
            <!-- text -->
            <div class="form-control mb-4">
                <input type="text" id="text" placeholder="Name..." v-model="text"
                    class="w-full border border-gray-300 rounded-md py-2 px-3 focus:outline-none focus:border-teal-500">
            </div>
            <!-- amount -->
            <div class="form-control mb-4">
                <input type="text" id="amount" placeholder="Enter amount (-expense, +income)..." v-model="amount"
                    class="w-full border border-gray-300 rounded-md py-2 px-3 focus:outline-none focus:border-teal-500">
            </div>

            <!-- the button -->
            <button type="submit" class=" bg-teal-500 text-white hover:bg-teal-600 focus:outline-none p-2 rounded-md">Add
                transaction</button>
        </form>
    </div>
</template>
  
