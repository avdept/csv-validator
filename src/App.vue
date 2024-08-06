<template>
  <div class="container mx-auto p-4">
    <h1 class="text-2xl font-bold mb-4">CSV Data Validator</h1>
    <FileInput @file-loaded="handleFileLoaded" />
    <DataTable v-if="csvData.length" :data="csvData" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import FileInput from './components/FileInput.vue';
import DataTable from './components/DataTable.vue';

const csvData = ref([]);

const handleFileLoaded = (data) => {
  debugger
  csvData.value = data;
};
</script>
