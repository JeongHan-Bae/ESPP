<template>
  <div class="w-full max-w-md space-y-4 relative z-10">
    <div class="text-center">
      <img
          class="w-48 h-48 rounded-xl object-cover mx-auto"
          :src="coverSrc"
          alt="cover"
      />
      <h2 class="text-xl font-semibold mt-2">{{ current.title || 'Unnamed Music' }}</h2>
      <h3 class="text-sm text-gray-500 dark:text-gray-400">{{ current.artist || 'Anonymous Artist' }}</h3>
    </div>

    <audio ref="audio" class="w-full" controls :src="audioSrc"></audio>

    <div class="flex justify-between items-center mt-2 gap-2">
      <PrevButton @click="prevTrack"/>
      <NextButton @click="nextTrack"/>
      <ModeSelector v-model="mode"/>
      <ThemeToggle />
      <BlackoutToggle @click="toggleBlackout"/>
    </div>

    <div
        v-if="blackout"
        @click="toggleBlackout"
        class="fixed inset-0 z-50 cursor-pointer"
        style="position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: black; z-index: 9999;"
    ></div>
  </div>
</template>

<script setup lang="ts">
import {ref, computed, onMounted, watch, nextTick} from 'vue'

import PrevButton from './icons/PrevButton.vue'
import NextButton from './icons/NextButton.vue'
import ModeSelector from './icons/ModeSelector.vue'
import ThemeToggle from './icons/ThemeToggle.vue'
import BlackoutToggle from './icons/BlackoutToggle.vue'

const playlist = ref<any[]>([])
const index = ref(0)
const mode = ref<'order' | 'reverse' | 'loop'>('order')
const audio = ref<HTMLAudioElement | null>(null)
const blackout = ref(false)
const originalTheme = ref<'light' | 'dark'>('light')

const current = computed(() => playlist.value[index.value] || {})
const audioSrc = computed(() => current.value.filename ? `/music/${current.value.filename}` : '')
const coverSrc = computed(() => current.value.cover ? `/images/${current.value.cover}` : '/images/default.png')

function nextTrack() {
  if (playlist.value.length === 0) return

  if (mode.value === 'loop') {
    if (audio.value) {
      audio.value.currentTime = 0
      audio.value.play()
    }
    return
  }

  const dir = mode.value === 'reverse' ? -1 : 1
  index.value = (index.value + dir + playlist.value.length) % playlist.value.length
  nextTick(() => audio.value?.play())
}

function prevTrack() {
  if (playlist.value.length === 0) return

  const dir = mode.value === 'reverse' ? 1 : -1
  index.value = (index.value + dir + playlist.value.length) % playlist.value.length
  nextTick(() => audio.value?.play())
}

function toggleBlackout() {
  blackout.value = !blackout.value

  if (blackout.value) {
    originalTheme.value = document.documentElement.classList.contains('dark') ? 'dark' : 'light'
    document.documentElement.classList.remove('dark')
    document.body.style.backgroundColor = 'black'
    if (!document.fullscreenElement) {
      document.documentElement.requestFullscreen().catch(console.warn)
    }
  } else {
    document.body.style.backgroundColor = ''
    if (originalTheme.value === 'dark') {
      document.documentElement.classList.add('dark')
    } else {
      document.documentElement.classList.remove('dark')
    }
    if (document.fullscreenElement) {
      document.exitFullscreen().catch(console.warn)
    }
  }
}

onMounted(async () => {
  try {
    const res = await fetch('/playlist.json')
    playlist.value = await res.json()
    if (audio.value) {
      audio.value.addEventListener('ended', () => {
        nextTrack()
      })
    }
  } catch (err) {
    console.error('Failed to load playlist.json:', err)
  }
})

watch(index, () => {
  if (audio.value) {
    audio.value.load()
    nextTick(() => audio.value?.play())
  }
})
</script>

<style scoped>
select {
  outline: none;
}
</style>
