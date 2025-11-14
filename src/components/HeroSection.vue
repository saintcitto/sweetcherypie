<template>
  <section ref="hero" class="hero">
    <video
      ref="videoRef"
      class="hero-video"
      :src="heroVideo"
      autoplay
      muted
      loop
      playsinline
    ></video>
    <div class="hero-overlay"></div>

    <div class="hero-content" ref="heroContent">
      <h1 class="title">For You, Chery</h1>
      <p class="subtitle">A space where my heart found its peace again.</p>
    </div>
  </section>
</template>

<script setup>
import DebugOnly from './DebugOnly.vue'
import { ref, onMounted } from 'vue'
import heroVideo from '../assets/story/chery1.mp4'
import gsap from 'gsap'

const hero = ref(null)
const heroContent = ref(null)
const videoRef = ref(null)

onMounted(() => {
  const el = hero.value
  const content = heroContent.value
  const video = videoRef.value
  if (!el || !content || !video) return

  // 1. Memperbaiki Autoplay
  const promise = video.play()
  if (promise !== undefined) {
    promise.catch(error => {
      console.error("Autoplay diblokir:", error)
      const playOnFirstClick = () => video.play()
      window.addEventListener('click', playOnFirstClick, { once: true })
      window.addEventListener('touchstart', playOnFirstClick, { once: true })
    })
  }

  // 2. Animasi Staggered Fade-in (Tetap sama)
  const title = content.querySelector('.title')
  const subtitle = content.querySelector('.subtitle')

  gsap.from([title, subtitle], {
    opacity: 0,
    y: 30,
    duration: 1.2,
    ease: 'power3.out',
    stagger: 0.2
  })

  // --- 3. LOGIKA "WOW" BARU (PARALLAX) ---
  const maxMove = 20 // Jarak gerak maksimum (px)

  el.addEventListener('mousemove', (e) => {
    const rect = el.getBoundingClientRect()
    const x = (e.clientX - rect.left - rect.width / 2) / (rect.width / 2)
    const y = (e.clientY - rect.top - rect.height / 2) / (rect.height / 2)
    
    // Mouse Y -> menggerakkan Y, Mouse X -> menggerakkan X
    const moveX = x * maxMove
    const moveY = y * maxMove

    // Gerakkan KONTEN sedikit (berlawanan arah mouse)
    gsap.to(content, {
      x: -moveX,
      y: -moveY,
      duration: 0.8,
      ease: 'power3.out'
    })
    
    // Gerakkan VIDEO lebih banyak (searah mouse)
    gsap.to(video, {
      x: moveX * 2, // Bergerak 2x lebih jauh
      y: moveY * 2,
      duration: 0.8,
      ease: 'power3.out'
    })
  })

  // Reset saat mouse keluar
  el.addEventListener('mouseleave', () => {
    gsap.to([content, video], {
      x: 0,
      y: 0,
      duration: 1.2,
      ease: 'elastic.out(1, 0.3)'
    })
  })
})
</script>

<style scoped>
.hero {
  position: relative;
  min-height: 100vh;
  background: #000;
  overflow: hidden; /* PENTING: Sembunyikan video yg 'bocor' */
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  text-align: center;
  color: #fff;
  padding: 2rem;
  /* Hapus 'perspective' karena kita tidak pakai rotasi lagi */
}

.hero-video {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: brightness(0.35) contrast(1.25) saturate(1.3);
  
  /* Perbesar video agar tidak ada tepi kosong saat bergerak */
  transform: scale(1.15); 
  
  pointer-events: none;
}

.hero-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(0,0,0,0.2), rgba(0,0,0,0.85));
  z-index: 1;
}

.hero-content {
  position: relative;
  z-index: 2;
  /* Hapus 'transform-style' */
}

.title {
  font-size: 3rem;
  font-weight: 600;
  text-shadow: 0 0 16px rgba(242,133,142,0.85);
}

.subtitle {
  margin-top: 0.6rem;
  font-size: 1.15rem;
  opacity: 0.9;
  font-style: italic;
}
</style>