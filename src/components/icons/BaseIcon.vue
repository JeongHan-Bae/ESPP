<template>
  <component
      :is="tag"
      :class="computedClass"
      v-bind="attrs"
  >
    <div class="inline-flex items-center justify-center min-w-[32px] min-h-[32px] w-[50%] h-[50%]">
      <slot />
    </div>
  </component>
</template>

<script setup lang="ts">
import { computed, useAttrs } from 'vue'

const props = defineProps<{
  tag?: string
  lightClass?: string
  darkClass?: string
  baseClass?: string
}>()

const attrs = useAttrs()

const computedClass = computed(() => {
  return [
    'relative inline-flex items-center justify-center',
    props.baseClass || '',
    props.lightClass || '',
    props.darkClass ? `dark:${props.darkClass}` : ''
  ]
})
</script>
