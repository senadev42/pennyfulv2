<script setup>
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

watch(() => groupedTransactions, (newTransactions) => {

    console.log(newTransactions);

}, { immediate: true });


const transactionClass = (amount) => {
    return amount >= 0 ? "text-green-500" : "text-red-500";
};

//deletion objects
let deleteemit = defineEmits(["transactionDeleted"]);
const onDelete = (target) => {
    deleteemit("transactionDeleted", target);
};
</script>

<template>
    <!-- Loop through grouped transactions -->
    <ul v-if="Object.keys(groupedTransactions).length > 0" id="list">
        <div v-for="(transactions, date) in groupedTransactions" :key="date"
            class="mb-5 bg-white p-4 rounded-lg shadow-sm">
            <h3 class="text-xs mb-2 text-gray-400">{{ date }}</h3>
            <!-- Display the date as a header for transactions grouped under it -->
            <div v-for="transaction in transactions" :key="transaction.id">
                <li class="grid grid-cols-8 gap-x-6 text-start border-t border-gray-200 py-2 text-sm md:text-base">

                    <!-- Transaction name -->
                    <span class="text-gray-800 col-span-4">{{ transaction.text }}</span>

                    <!-- Transaction amount -->
                    <span :class="transactionClass(transaction.amount)" class="col-span-2">
                        ${{ Math.abs(transaction.amount) }}
                    </span>

                    <!-- More info -->
                    <button class="delete-btn focus:outline-none col-span-1" @click="moreinfo(transaction.id)">
                        <!-- <i class="pi pi-info-circle text-teal-300 hover:text-teal-800" /> -->
                    </button>

                    <!-- Delete -->
                    <button class="delete-btn focus:outline-none col-span-1" @click="onDelete(transaction.id)">
                        <i class="pi pi-trash text-teal-300 hover:text-teal-800" />
                    </button>
                </li>
            </div>
        </div>
    </ul>

    <div v-else class="text-gray-500 text-sm px-14 text-center">
        Nothing for now.
    </div>
</template>
