<script setup>

import { ref, computed } from 'vue';

// form data
const text = ref('');
const amount = ref('');
const category = ref('');
const date = ref('');
const paymentType = ref('');





const buttonStyle = computed(() => {
    return amount.value > 0 ? {
        actualStyle: "border-b border-green-400 text-green-400 hover:text-black",
        text: "Add Inflow"
    } : {
        actualStyle: "border-b border-red-400 text-red-400 hover:text-black hover:bg-red-400",
        text: "Subtract Expense"
    }

})

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
};

const clearform = () => {
    text.value = '';
    amount.value = '';
}



// categories
const categories = ['Food', 'Housing', 'Utilities', 'Transportation', 'Entertainment', 'Health', 'Education', 'Gifts', 'Miscellaneous', 'Savings'];
const paymentTypes = ['Credit Card', 'Debit Card', 'Cash', 'Bank Transfer'];

</script>

<template>
    <div class="mt-2">
        <form id="form" @submit.prevent="onSubmit"
            class="bg-white p-4 rounded-md shadow-md text-xs md:text-sm flex flex-col items-stretch">

            <!-- one row -->
            <div class="flex flex-col md:flex-row gap-x-2 ">
                <!-- text -->
                <div class="form-control mb-4 w-full">
                    <input type="text" id="text" placeholder="Groceries" v-model="text"
                        class="w-full border border-gray-300 rounded-md py-2 px-2 focus:outline-none focus:border-teal-500" />
                </div>
                <!-- amount -->
                <div class="form-control mb-4 w-full ">
                    <input type="text" id="amount" placeholder="-13.50" v-model="amount"
                        class="w-full border border-gray-300 rounded-md py-2 px-2 focus:outline-none focus:border-teal-500" />
                </div>

            </div>

            <!-- one row -->
            <!-- <div class="flex flex-col md:flex-row gap-x-2 justify-around">

                <div class="form-control mb-4 w-full">
                    <select id="category" v-model="category"
                        class="w-full border border-gray-300 rounded-md py-2 px-3 focus:outline-none focus:border-teal-500">
                        <option value="" disabled selected>Category</option>
                        <option v-for="cat in categories" :key="cat">{{ cat }}</option>
                    </select>
                </div>

                <div class="form-control mb-4 w-full">
                    <input type="date" id="date" v-model="date"
                        class="w-full border border-gray-300 rounded-md py-2 px-3 focus:outline-none focus:border-teal-500" />
                </div>

                <div class="form-control mb-4 w-full">
                    <select id="paymentType" v-model="paymentType"
                        class="w-full border border-gray-300 rounded-md py-2 px-3 focus:outline-none focus:border-teal-500">
                        <option value="" disabled selected>Payment</option>
                        <option v-for="payment in paymentTypes" :key="payment">{{ payment }}</option>
                    </select>
                </div>
            </div> -->


            <div class="grid grid-cols-4 gap-x-1">
                <button type="submit" class="text-white focus:outline-none p-2 col-span-3"
                    :class="buttonStyle.actualStyle">{{
                        buttonStyle.text }}</button>
                <button @click="clearform"
                    class=" border-y-slate-500 border-black text-slate-400 focus:outline-none p-2 col-span-1 rounded-md">Clear</button>
            </div>
            <!-- the button -->

        </form>
    </div>
</template>
  
