<template>
  <section ref="hero" class="hero">
    <div class="hero-overlay"></div>

    <div class="hero-img-wrapper">
      <img src="../assets/chery.jpeg" alt="Chery" class="hero-img" />
    </div>

    <h1 class="title">For You, Chery</h1>
    <p class="subtitle">A space where my heart found its peace again.</p>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'

const hero = ref(null)

onMounted(() => {
  // Animasi awal masuk
  gsap.from(hero.value.querySelector('.hero-img-wrapper'), {
    scale: 0.8,
    opacity: 0,
    duration: 1.8,
    ease: 'power3.out'
  })

  gsap.from(hero.value.querySelector('.title'), {
    opacity: 0,
    y: 30,
    delay: 0.8,
    duration: 1.5,
    ease: 'power2.out'
  })

  gsap.from(hero.value.querySelector('.subtitle'), {
    opacity: 0,
    y: 20,
    delay: 1.3,
    duration: 1.8,
    ease: 'power2.out'
  })

  // Efek zoom lembut di foto (slow breathing)
  gsap.to(hero.value.querySelector('.hero-img'), {
    scale: 1.05,
    duration: 6,
    yoyo: true,
    repeat: -1,
    ease: 'sine.inOut'
  })

  // Animasi gradasi background bergerak
  gsap.to('.hero', {
    backgroundPosition: '200% center',
    duration: 12,
    repeat: -1,
    yoyo: true,
    ease: 'sine.inOut'
  })
})
</script>

<style scoped>
.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: linear-gradient(120deg, #7a2246, #000, #f2858e);
  background-size: 300% 300%;
  text-align: center;
  padding: 2rem;
  overflow: hidden;
  color: #fff;
}

/* Lapisan kabut/gelap lembut */
.hero-overlay {
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at center, rgba(0, 0, 0, 0.4), rgba(10, 0, 0, 0.9));
  z-index: 0;
}

/* Bungkus agar crop lingkaran sempurna */
.hero-img-wrapper {
  position: relative;
  width: 200px;
  height: 200px;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 0 25px rgba(242, 133, 142, 0.4);
  margin-bottom: 1.2rem;
  z-index: 1;
}

/* Pastikan gambar proporsional */
.hero-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  filter: brightness(0.95) contrast(1.1);
  transition: filter 1s ease;
}

/* Efek saat hover */
.hero-img-wrapper:hover .hero-img {
  filter: brightness(1.05) contrast(1.2) saturate(1.1);
}

.title {
  font-size: 2.8rem;
  font-weight: 600;
  z-index: 1;
  text-shadow: 0 0 15px rgba(242, 133, 142, 0.5);
  letter-spacing: 1px;
}

.subtitle {
  margin-top: 0.8rem;
  font-size: 1.1rem;
  opacity: 0.85;
  z-index: 1;
  font-style: italic;
  animation: fadePulse 4s ease-in-out infinite;
}

/* Animasi glow pelan di subtitle */
@keyframes fadePulse {
  0%, 100% {
    text-shadow: 0 0 6px rgba(242, 133, 142, 0.4);
  }
  50% {
    text-shadow: 0 0 16px rgba(242, 133, 142, 0.8);
  }
}
</style>