<template>
  <div class="flex w-fit">
    <div v-for="(item, index) in compNameItems" :key="index" class="flex items-center me-4">
      <input :id="item" type="radio" v-model="selectedComp" :value="item" name="inline-radio-group" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
      <label :for="item" class="ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">{{ item }}</label>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, defineEmits } from 'vue'
import {compNameItems} from "../constants.ts";

const selectedComp = ref(compNameItems[0])
const emit = defineEmits(['on-select-comp'])

watch(selectedComp, (newVal) => {
  emit('on-select-comp', newVal)
})
</script>

<style scoped>
</style>
