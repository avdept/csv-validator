<template>
  <div class="mb-4">
    <input
      type="file"
      accept=".csv"
      @change="handleFileUpload"
      class="hidden"
      ref="fileInput"
    />
    <input
      v-model="fileUrl"
      @keyup.enter="handleUrlInput"
      placeholder="Enter CSV file URL"
      class="border p-2 mr-2"
    />
    <button
      @click="$refs.fileInput.click()"
      class="bg-blue-500 text-white px-4 py-2 rounded"
    >
      Upload CSV
    </button>
    <button
      @click="handleUrlInput"
      class="bg-green-500 text-white px-4 py-2 rounded ml-2"
    >
      Load from URL
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Papa from 'papaparse';

const emit = defineEmits(['file-loaded']);
const fileUrl = ref('');
const fileInput = ref(null);

const handleFileUpload = (event) => {
  const file = event.target.files[0];
  if (file) {
    Papa.parse(file, {
      skipEmptyLines: true,
      complete: (results) => {
        emit('file-loaded', results.data);
      },
      header: true,
    });
  }
};

const handleUrlInput = async () => {
  if (fileUrl.value) {
    try {
      const response = await fetch(fileUrl.value);
      const csvText = await response.text();
      Papa.parse(csvText, {
        complete: (results) => {
          emit('file-loaded', results.data);
        },
        header: true,
      });
    } catch (error) {
      console.error('Error loading CSV from URL:', error);
    }
  }
};
</script>
