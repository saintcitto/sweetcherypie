<template>
  <div class="app">
    <HeroSection />
    <StorySection />
    <GallerySection />
    <FooterSection />

    <!-- Audio autoplay -->
    <audio ref="bgMusic" src="../src/assets/afterhours.mp3" autoplay loop></audio>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import HeroSection from './components/HeroSection.vue'
import StorySection from './components/StorySection.vue'
import GallerySection from './components/GallerySection.vue'
import FooterSection from './components/FooterSection.vue'

const bgMusic = ref(null)

onMounted(() => {
  const playMusic = () => {
    bgMusic.value
      ?.play()
      .then(() => console.log('🎧 Musik berhasil diputar'))
      .catch(() => {
        console.warn('Autoplay diblokir browser — menunggu klik pertama...')
        document.addEventListener(
          'click',
          () => {
            bgMusic.value.play()
          },
          { once: true }
        )
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