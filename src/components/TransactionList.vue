<script setup>


const props = defineProps({
    transactions: {
        type: Array,
        required: true,
    },
});

const transactionClass = (amount) => {
    return amount >= 0 ? "text-green-500" : "text-red-500";
};

//deletion objects
let deleteemit = defineEmits(['transactionDeleted']);

const onDelete = (target) => {
    deleteemit('transactionDeleted', target);
}


</script>

<template>
    <!-- loop through transactions -->
    <ul v-if="transactions.length > 0" id="list" class="bg-white p-4 rounded-lg shadow-sm">
        <li v-for="transaction in transactions" :key="transaction.id"
            class="grid grid-cols-7 gap-x-6 text-start border-b border-gray-200 py-2 text-sm md:text-base">
            <span class="text-gray-800 col-span-2">{{ transaction.text }}</span>

            <span :class="transactionClass(transaction.amount)" class="col-span-2">
                ${{ transaction.amount }}
            </span>

            <span class="text-gray-700 col-span-2">{{ transaction.date }}</span>

            <button class="delete-btn focus:outline-none col-span-1" @click="onDelete(transaction.id)">
                <i class="pi pi-trash text-teal-300 hover:text-teal-800" />
            </button>
        </li>

    </ul>
    <div v-else class="text-gray-500 text-sm px-14">
        Nothing for now.
    </div>
</template>
