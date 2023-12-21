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
let deleteemit = defineEmits(["transactionDeleted"]);

const onDelete = (target) => {
    deleteemit("transactionDeleted", target);
};
</script>

<template>
    <!-- loop through transactions -->
    <ul v-if="transactions.length > 0" id="list" class="bg-white p-4 rounded-lg shadow-sm">
        <div v-for="transaction in transactions" :key="transaction.id">
            <li class="grid grid-cols-8 gap-x-6 text-start border-b border-gray-200 py-2 text-sm md:text-base">


                <span class="text-gray-800 col-span-4">{{ transaction.text }}</span>

                <span :class="transactionClass(transaction.amount)" class="col-span-2">
                    ${{ transaction.amount < 0 ? transaction.amount * -1 : transaction.amount }} </span>

                        <!-- more info -->
                        <button class="delete-btn focus:outline-none col-span-1" @click="moreinfo(transaction.id)">
                            <!-- <i class="pi pi-info-circle text-teal-300 hover:text-teal-800" /> -->
                        </button>
                        <!-- delete -->
                        <button class="delete-btn focus:outline-none col-span-1" @click="onDelete(transaction.id)">
                            <i class="pi pi-trash text-teal-300 hover:text-teal-800" />
                        </button>
            </li>
            <!-- <li class="grid grid-cols-3 gap-x-2 text-center bg-zinc-200 py-2 text-sm md:text-base">
                <div class="col-span-1">
                    <p>{{ transaction.date }}</p>
                </div>
                <div class="flex flex-row">
                    <p>{{ transaction.category }}</p>
                    <p>{{ transaction.paymentType }}</p>
                </div>
            </li> -->
        </div>

    </ul>
    <div v-else class="text-gray-500 text-sm px-14 text-center">
        Nothing for now.
    </div>
</template>
