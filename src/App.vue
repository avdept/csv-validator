<template>
  <div class="container mx-auto p-4">
    <div class="flex justify-between items-start">
      <h1 class="text-2xl font-bold mb-4">Free online CSV data validator</h1>
      <div class="flex flex-col gap-2">
        <div class="flex gap-2">
          <a href="https://twitter.com/_avdept" class="twitter-follow-button" data-size="large" data-show-count="false">Follow or contact on Twitter</a>
          <a href="https://www.buymeacoffee.com/avdept" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 50px !important;width: 217px !important;" ></a>

        </div>
        <span class="text-xs text-gray-400 max-w-96">This was made as a prototype during my lunch time. I've added few basic validations, but if you need more, feel free to contact me via Twitter</span>
      </div>
    </div>
    <p class="mb-4"><b>Privacy notice:</b> The data you upload does not leave your browser as this app does not have any backends.</p>
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
  csvData.value = data;
};
</script>
