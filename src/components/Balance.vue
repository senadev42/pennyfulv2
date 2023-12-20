<script setup>

const props = defineProps({
    total: {
        type: Number,
        required: true,
    },
});
</script>

<template>
    <div class="text-center mt-2">
        <h4 class="text-gray-600 text-lg mb-2">Your Balance</h4>
        <h1 id="balance" class="text-4xl font-bold text-teal-500">${{ total }}</h1>
    </div>
</template>
  