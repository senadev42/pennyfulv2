<script setup lang="ts">
defineProps<{
    exportdata: (livetransactions: any) => void;
    transactions: any[];
    changedatasource: () => void;
}>()
const datasource = defineModel<string>('datasource', { required: true })
</script>

<template>
    <div class="text-xs flex flex-row items-start justify-start gap-x-2 mr-2 ">
        <!-- export  -->
        <div class="flex gap-x-2 items-start">
            <button @click="exportdata(transactions)">
                <i class="pi text-teal-300 hover:text-teal-800 pi-save" />
            </button>
            <p>
                export to csv
            </p>

        </div>

        <!-- data source -->
        <div class="flex gap-x-2 items-start">
            <button @click="changedatasource">
                <i class="pi text-teal-300 hover:text-teal-800 "
                    :class="datasource == 'mock' ? 'pi-chevron-up' : 'pi-chevron-down'" />
            </button>
            <p>
                datasource: {{ datasource }}
            </p>
        </div>
    </div>
</template>
