<script setup lang="ts">
import { computed, watch, toRaw } from 'vue';

const props = defineProps({
    transactions: {
        type: Array,
        required: true,
    },
});

const groupedTransactions = computed(() => {
    const grouped = {};
    props.transactions.forEach(transaction => {
        const date = transaction.date;
        if (!grouped[date]) {
            grouped[date] = [];
        }
        grouped[date].push(transaction);
    });
    return grouped;
});


//utility
const transactionClass = (amount) => {
    return amount >= 0 ? "text-green-500" : "text-red-500";
};

function formatDate(dateString) {
    const dateObj = new Date(dateString);
    return new Intl.DateTimeFormat('default', { dateStyle: 'long' }).format(dateObj);
}

//deletion objects
let deleteemit = defineEmits(["transactionDeleted"]);
const onDelete = (target) => {
    deleteemit("transactionDeleted", target);
};
</script>

<template>
    <!-- Loop through grouped transactions -->
    <ul v-if="Object.keys(groupedTransactions).length > 0" id="list" class="min-h-screen">

        <div v-for="(transactions, date) in groupedTransactions" :key="date"
            class=" bg-white rounded-lg shadow-sm mb-2 p-2">


            <h3 class="text-xs mb-2 text-gray-400">{{ formatDate(date) }}</h3>

            <div v-for="transaction in transactions" :key="transaction.id">
                <li class="grid grid-cols-4 gap-x-6 py-1 text-start border-t border-gray-200 text-sm md:text-base">

                    <span class="text-gray-800 col-span-2">{{ transaction.text }}</span>

                    <span :class="transactionClass(transaction.amount)" class="col-span-1">
                        ${{ Math.abs(transaction.amount) }}
                    </span>

                    <button class="delete-btn focus:outline-none col-span-1" @click="onDelete(transaction.id)">
                        <i class="pi pi-trash text-teal-300 hover:text-teal-800" />
                    </button>
                </li>
            </div>
        </div>
    </ul>

    <div v-else class="text-gray-500 text-sm px-14 text-center ">
        Nothing for now.
    </div>
</template>
