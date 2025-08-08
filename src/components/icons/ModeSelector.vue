<template>
  <div
      class="flex justify-between items-center p-2 border border-gray-300 rounded
           bg-white dark:bg-gray-900 w-full max-w-[40%] min-w-[120px] overflow-hidden"
  >
    <button
        v-for="opt in options"
        :key="opt.value"
        :title="opt.label"
        @click="changeMode(opt.value)"
        class="flex-1 min-w-0 mx-1 p-1 rounded transition-colors duration-200 flex justify-center items-center"
        :class="modelValue === opt.value
        ? 'bg-orange-100 dark:bg-blue-600'
        : 'hover:bg-gray-200 dark:hover:bg-gray-700'"
    >
      <component
          :is="opt.icon"
          class="w-[80%] h-[80%] max-w-8 max-h-8"
          :class="modelValue === opt.value
          ? 'text-black dark:text-blue-200'
          : 'text-gray-800 dark:text-gray-200'"
      />
    </button>
  </div>
</template>


<script setup lang="ts">
import OrderIcon from '@/assets/Order.svg'
import ReverseIcon from '@/assets/Reverse.svg'
import LoopIcon from '@/assets/Loop.svg'

type Mode = 'order' | 'reverse' | 'loop'

defineProps<{
  modelValue: Mode
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: Mode): void
}>()

function changeMode(value: Mode) {
  emit('update:modelValue', value)
}

const options: { value: Mode; icon: any; label: string }[] = [
  { value: 'order', icon: OrderIcon, label: 'Order' },
  { value: 'reverse', icon: ReverseIcon, label: 'Reverse' },
  { value: 'loop', icon: LoopIcon, label: 'Loop' },
]
</script>
