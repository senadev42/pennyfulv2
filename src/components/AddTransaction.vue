<script setup>

import { ref, computed } from 'vue';

const text = ref('');
const amount = ref('');

const buttonStyle = computed(() => {
    return amount.value > 0 ? {
        actualStyle: "bg-green-400 hover:bg-green-500",
        text: "Add Inflow"
    } : {
        actualStyle: "bg-red-400 hover:bg-red-500",
        text: "Subtract Expense"
    }

})

let submitemit = defineEmits(['transactionSubmitted']);

const onSubmit = () => {
    // console.log(buttonStyle.value.actualStyle)
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

const clearform = () => {
    text.value = '';
    amount.value = '';
}



</script>

<template>
    <div class="mt-2">
        <form id="form" @submit.prevent="onSubmit"
            class="bg-white p-4 rounded-md shadow-md text-xs md:text-sm flex flex-col items-stretch">
            <!-- text -->
            <div class="form-control mb-4">
                <input type="text" id="text" placeholder="Groceries" v-model="text"
                    class="w-full border border-gray-300 rounded-md py-2 px-3 focus:outline-none focus:border-teal-500">
            </div>
            <!-- amount -->
            <div class="form-control mb-4">
                <input type="text" id="amount" placeholder="-13" v-model="amount"
                    class="w-full border border-gray-300 rounded-md py-2 px-3 focus:outline-none focus:border-teal-500">
            </div>

            <div class="grid grid-cols-4 gap-x-1 w-[20rem]">
                <button type="submit" class="text-white focus:outline-none p-2 rounded-md col-span-3"
                    :class="buttonStyle.actualStyle">{{
                        buttonStyle.text }}</button>
                <button @click="clearform" class="text-white bg-slate-400 focus:outline-none p-2 col-span-1">Clear</button>
            </div>
            <!-- the button -->

        </form>
    </div>
</template>
  
