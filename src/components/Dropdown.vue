<script setup>
import { computed, ref } from 'vue'

  const props = defineProps({
    items: {
      type: Array,
      required: true,
    },
    multiple: {
      type: Boolean,
      default: false,
    },
    placeholder: {
      type: String,
      default: 'Select an option',
    },
    align: {
        type: String,
        default: 'left',
    },
    contentClasses: {
        type: Array,
        default: () => ['py-1', 'bg-white dark:bg-gray-700'],
    },
  });

  const emit = defineEmits(['dropdownValue']);

  const selectedItems = ref([]);
  const isOpen = ref(false);

  const closeOnEscape = (e) => {
      if (isOpen.value && e.key === 'Escape') {
        isOpen.value = false
      }
  };
  const toggleDropdown = () => {
    isOpen.value = !isOpen.value;
  };

  const selectItem = (item) => {
    if (props.multiple) {
      if (selectedItems.value.some((i) => i.title === item.title)) {
        selectedItems.value = selectedItems.value.filter((i) => i.title !== item.title);
      } else {
        selectedItems.value = [...selectedItems.value, item];
      }
    } else {
      selectedItems.value = [item];
      isOpen.value = true;
    }
    const selectedValues = selectedItems.value.map((selectedItem) => selectedItem.value);
    
    emit('dropdownValue', props.multiple ? selectedValues : selectedValues[0]);
  };

  const alignmentClasses = computed(() => {
    if (props.align === 'left') {
        return 'origin-top-left left-0'
    }

    if (props.align === 'right') {
        return 'origin-top-right right-0'
    }
    return 'origin-top'
  })



</script>
<template>
  <div class="relative border-2 border-slate-400 rounded-lg px-2 min-w-[250px] max-w-[250px] min-h-[30px]" @click="toggleDropdown">
      <div class="cursor-pointer">
        {{ selectedItems.length > 0 ? selectedItems.map(item => item.title).join(', ') : placeholder }}
      </div>

    <transition name="fade"
          enter-active-class="transition ease-out duration-200"
          enter-from-class="transform opacity-0 scale-95"
          enter-to-class="transform opacity-100 scale-100"
          leave-active-class="transition ease-in duration-75"
          leave-from-class="transform opacity-100 scale-100"
          leave-to-class="transform opacity-0 scale-95"
    >
      <div v-if="isOpen" :class="[alignmentClasses]" class="absolute z-10 w-full mt-2 bg-white border rounded-md shadow-lg px-2 cursor-pointer">
        <ul>
          <li v-for="(item, index) in items" :key="index" @click="selectItem(item)"  clas="pb-4">
            {{ item.title }}
          </li>
        </ul>
      </div>
    </transition>
  </div>
</template>
<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
