<template>
  <div class="overflow-x-auto">
    <table class="min-w-full bg-white table-auto">
      <thead>
        <tr>
          <th
            v-for="(column, index) in columns"
            :key="index"
            class="border px-4 py-2"
          >
            <div class="flex justify-between">
              <span>
                {{ column }}
              </span>
              <div class="mb-2 relative">
                <button
                  @click="toggleDropdown(column)"
                  class="bg-gray-200 px-2 py-1 rounded focus:outline-none"
                >
                  Validations ▼
                </button>
                <div
                  v-if="openDropdown === column"
                  class="absolute z-10 bg-white border rounded mt-1 py-1 w-48 text-left"
                >
                  <label class="block px-4 py-2">
                    <input
                      type="checkbox"
                      v-model="validationRules[column].required"
                      @change="forceRerender"
                      class="mr-2"
                    />
                    Required
                  </label>
                  <label class="block px-4 py-2">
                    <input
                      type="checkbox"
                      v-model="validationRules[column].numeric"
                      @change="forceRerender"
                      class="mr-2"
                    />
                    Numeric
                  </label>

                  <label class="block px-4 py-2">
                    <input
                      type="checkbox"
                      v-model="validationRules[column].alphabetic"
                      @change="forceRerender"
                      class="mr-2"
                    />
                    Alphabetic
                  </label>
                </div>
              </div>
            </div>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rowIndex) in data" :key="rowIndex">
          <td
            v-for="(column, colIndex) in columns"
            :key="colIndex"
            class="px-4 py-2"
            :class="{
              'border-red-500 border-2': isInvalid(
                row[column],
                validationRules[column]
              ),
              border: !isInvalid(row[column], validationRules[column]),
            }"
          >
            {{ row[column] }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, computed, reactive } from "vue";

const props = defineProps({
  data: {
    type: Array,
    required: true,
  },
});

const columns = computed(() => {
  return props.data.length > 0 ? Object.keys(props.data[0]) : [];
});

const validationRules = reactive({});
const openDropdown = ref(null);
const rerenderTrigger = ref(0);

columns.value.forEach((column) => {
  validationRules[column] = {
    required: false,
    numeric: false,
    alphabetic: false,
  };
});

const toggleDropdown = (column) => {
  if (openDropdown.value === column) {
    openDropdown.value = null;
  } else {
    openDropdown.value = column;
  }
};

const isNumeric = (value) => {
  return /^-?\d*\.?\d+$/.test(value);
};

const isAlphabetic = (value) => {
  return /^[A-Za-z]+$/.test(value);
};

const isInvalid = (value, rules) => {
  if (
    rules.required &&
    (value === null || value === undefined || value === "")
  ) {
    return true;
  }
  if (rules.numeric && !isNumeric(value)) {
    return true;
  }
  if (rules.alphabetic && !isAlphabetic(value)) {
    return true;
  }
  return false;
};

const forceRerender = () => {
  rerenderTrigger.value += 1;
};
</script>
