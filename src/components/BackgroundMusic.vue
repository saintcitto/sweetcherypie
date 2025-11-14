<script setup>
import DebugOnly from './DebugOnly.vue'
</script>

<template>
  <audio ref="audioRef" src="assets/afterhours.mp3" loop></audio>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const audioRef = ref(null)
const isPlaying = ref(false)

onMounted(() => {
  const audio = audioRef.value
  if (!audio) return
  audio.volume = 0.6

  const tryPlay = async () => {
    try {
      await audio.play()
      isPlaying.value = true
    } catch {
      console.log('Autoplay diblokir browser. Menunggu interaksi user...')
    }
  }

  tryPlay()

  const resumeOnClick = async () => {
    if (!isPlaying.value) {
      try {
        await audio.play()
        isPlaying.value = true
        window.removeEventListener('click', resumeOnClick)
      } catch (err) {
        console.log('Gagal memulai audio:', err)
      }
    }
  }

  window.addEventListener('click', resumeOnClick)
})
</script>