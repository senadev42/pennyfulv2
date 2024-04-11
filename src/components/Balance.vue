<script setup>

const props = defineProps({
    total: {
        type: Number,
        required: true,
    },
});

const transactionClass = (amount) => {
    return amount >= 0 ? "text-green-500" : "text-red-500";
};


</script>

<template>
    <div class="text-center mt-2">
        <h4 class="text-gray-600 text-lg mb-2">Your Balance</h4>
        <h1 id="balance" :class="transactionClass(total)" class="text-4xl font-bold ">${{ total }}</h1>
    </div>
</template>