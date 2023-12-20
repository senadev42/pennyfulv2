<script setup>

const props = defineProps({
    income: {
        type: Number,
        required: true,
    },
    expenses: {
        type: Number,
        required: true,
    },
});

</script>


<template>
    <div class="flex justify-around rounded-lg gap-x-6 bg-zinc-200 p-4 mt-4">
        <div class="text-center">
            <h4 class="text-gray-600 text-sm mb-1">Income</h4>
            <p id="money-plus" class="text-green-500 font-semibold text-lg">+${{ income }}</p>
        </div>
        <div class="text-center">
            <h4 class="text-gray-600 text-sm mb-1">Expenses</h4>
            <p id="money-minus" class="text-red-500 font-semibold text-lg">-${{ expenses * -1 }}</p>
        </div>
    </div>
</template>
  