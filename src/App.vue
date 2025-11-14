<template>
  <div class="app">
    <HeroSection />
    <StorySection />
    <GallerySection />
    <FooterSection />

    <audio ref="bgMusic" :src="musicSrc" loop></audio>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import HeroSection from './components/HeroSection.vue'
import StorySection from './components/StorySection.vue'
import GallerySection from './components/GallerySection.vue'
import FooterSection from './components/FooterSection.vue'

import afterhours from './assets/afterhours.mp3'

const bgMusic = ref(null)
const musicSrc = afterhours

onMounted(() => {
  const playMusic = () => {
    const audio = bgMusic.value
    if (!audio) return

    audio.volume = 0.6

    audio
      .play()
      .then(() => {
        console.log('🎧 Musik berhasil diputar otomatis')
      })
      .catch(() => {
        console.warn('⚠️ Autoplay diblokir browser — menunggu klik pertama...')
        const onUserInteract = () => {
          audio.play().catch(err => console.error('Gagal memulai musik:', err))
          document.removeEventListener('click', onUserInteract)
        }
        document.addEventListener('click', onUserInteract, { once: true })
      })
  }

  playMusic()
})
</script>

<style>
.app {
  overflow-x: hidden;
  background-color: #000;
  color: #fff;
  font-family: 'Poppins', sans-serif;
}
</style>