<script setup>
import { ref } from 'vue';
import Dropdown from './components/Dropdown.vue'
const multiCheck = ref(false);
const showValue = ref([]);
const dropdownItems = [
  { title: 'Option 1', value: 'option1' },
  { title: 'Option 2', value: 'option2' },
  { title: 'Option 3', value: 'option3' },
  { title: 'Option 4', value: 'option4' },
  { title: 'Option 5', value: 'option5' },
];
const handleMultiSelect = (event) => {
  multiCheck.value = event.target.checked;
}
const selectedAction = (event) => {
  showValue.value = event;
}
</script>

<template>
  <header>
    
  </header>

  <main class="min-h-screen bg-gray-100">
    <div class="flex justify-center flex-col flex-wrap items-center min-h-screen">
      <div>
        <h3 class="font-bold pb-2">Dropdown Component</h3>
        <div class="relative flex gap-x-3">
          <div class="flex h-6 items-center">
            <input id="comments" name="comments" @change="handleMultiSelect($event)" type="checkbox" class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-600">
          </div>
          <div class="text-sm leading-6">
            <label for="comments" class="font-medium text-gray-900">Multiple</label>
          </div>
        </div>
        <Dropdown :items="dropdownItems" :multiple="multiCheck" @dropdownValue="selectedAction($event)"  align="left" />
        <h4 class="block">Selected Value: {{ showValue }}</h4>
      </div>
    </div>
  </main>
</template>

<style scoped>

</style>
