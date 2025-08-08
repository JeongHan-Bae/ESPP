<template>
  <button
      @click="toggleTheme"
      class="p-2 w-10 h-10 rounded transition-colors duration-300"
      :aria-label="theme === 'dark' ? 'Switch to light mode' : 'Switch to dark mode'"
  >
    <component
        :is="theme === 'dark' ? MoonIcon : SunIcon"
        class="w-full h-full"
    />
  </button>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

import SunIcon from '@/assets/Sun.svg'
import MoonIcon from '@/assets/Moon.svg'

type Theme = 'light' | 'dark'
const theme = ref<Theme>('light')

function applyTheme(newTheme: Theme) {
  document.documentElement.classList.toggle('dark', newTheme === 'dark')
  localStorage.setItem('theme', newTheme)
  theme.value = newTheme
}

function toggleTheme() {
  const next = theme.value === 'dark' ? 'light' : 'dark'
  applyTheme(next)
}

onMounted(() => {
  const stored = localStorage.getItem('theme') as Theme | null
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches
  const initial = stored ?? (prefersDark ? 'dark' : 'light')
  applyTheme(initial)
})
</script>
