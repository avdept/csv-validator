<template>
  <div class="overflow-x-auto">
    <table class="min-w-full bg-white">
      <thead>
        <tr>
          <th
            v-for="(column, index) in columns"
            :key="index"
            class="border px-4 py-2"
          >
            <div class="mb-2">
              <div class="flex justify-between items-start">
                <span>
                  {{ column }}
                </span>
                <div class="relative">
                  <button
                    @click="toggleDropdown(column)"
                    class="bg-gray-200 px-2 py-1 rounded focus:outline-none relative"
                  >
                    Validations ▼
                  </button>
                  <div
                    v-if="openDropdown === column"
                    class="absolute z-10 bg-white border rounded mt-1 py-1 w-48"
                  >
                    <label
                      v-for="(validator, key) in validators"
                      :key="key"
                      class="block px-4 py-2 text-left"
                    >
                      <input
                        type="checkbox"
                        v-model="validationRules[column][key]"
                        class="mr-2"
                      />
                      {{ validator.label }}
                    </label>
                  </div>
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
import { ref, computed, reactive, watch } from "vue";
import { RecycleScroller } from 'vue-virtual-scroller';
import 'vue-virtual-scroller/dist/vue-virtual-scroller.css';


const props = defineProps({
  data: {
    type: Array,
    required: true,
  },
});

const columns = computed(() => {
  return props.data.length > 0 ? Object.keys(props.data[0]) : [];
});

const validators = {
  required: {
    label: "Required",
    validate: (value) => value !== null && value !== undefined && value !== "",
  },
  numeric: {
    label: "Numeric",
    validate: (value) => /^-?\d*\.?\d+$/.test(value),
  },
  alphabetic: {
    label: "Alphabetic",
    validate: (value) => /^[A-Za-z]+$/.test(value),
  },
};

const validationRules = reactive({});
const openDropdown = ref(null);
const updateTrigger = ref(0);

columns.value.forEach((column) => {
  validationRules[column] = reactive(
    Object.keys(validators).reduce((acc, key) => {
      acc[key] = false;
      return acc;
    }, {})
  );
});

watch(
  validationRules,
  () => {
    updateTrigger.value += 1;
  },
  { deep: true }
);

const toggleDropdown = (column) => {
  if (openDropdown.value === column) {
    openDropdown.value = null;
  } else {
    openDropdown.value = column;
  }
};

const isInvalid = (value, rules) => {
  return Object.entries(rules).some(([key, isActive]) => {
    return isActive && !validators[key].validate(value);
  });
};
</script>

<style scoped>
.scroller {
  height: 400px; /* Adjust this value based on your needs */
}
</style>
