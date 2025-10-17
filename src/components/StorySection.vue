<template>
  <section ref="section" class="story">
    <div class="overlay"></div>

    <div class="content">
      <h2>Our Story</h2>
      <p>
        Every heartbeat and every silence had its own meaning.<br />
        These moments remind me of how love taught me peace again.
      </p>
    </div>

    <div class="photos">
      <img src="../assets/story/chery1.jpeg" alt="Chery 1" class="photo" />
      <img src="../assets/story/chery2.jpeg" alt="Chery 2" class="photo" />
      <img src="../assets/story/chery3.jpeg" alt="Chery 3" class="photo" />
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const section = ref(null)

onMounted(() => {
  if (!section.value) return
  const el = section.value
  const photos = el.querySelectorAll('.photo')

  // Fade in content
  gsap.from(el.querySelector('.content'), {
    scrollTrigger: {
      trigger: el,
      start: 'top 85%',
      toggleActions: 'play none none none',
    },
    opacity: 0,
    y: 60,
    duration: 1.4,
    ease: 'power3.out',
  })

  // Animasi foto satu per satu
  photos.forEach((photo, i) => {
    gsap.from(photo, {
      scrollTrigger: {
        trigger: el,
        start: 'top 80%',
        toggleActions: 'play none none none',
      },
      opacity: 0,
      scale: 0.9,
      y: 80,
      rotation: (i - 1) * 5,
      duration: 1.4,
      ease: 'power3.out',
      delay: i * 0.25,
    })
  })

  // Parallax lembut saat scroll
  gsap.to(photos, {
    y: (i) => (i % 2 === 0 ? 40 : -40),
    ease: 'none',
    scrollTrigger: {
      trigger: section.value,
      start: 'top bottom',
      end: 'bottom top',
      scrub: true,
    },
  })
})
</script>

<style scoped>
.story {
  position: relative;
  min-height: 100vh;
  padding: 5rem 2rem;
  background: linear-gradient(180deg, #000, #1a0f14 80%);
  color: #f5f5f5;
  overflow: hidden;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/* ✨ Lapisan gelap lembut dengan efek vignette */
.overlay {
  content: "";
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at center, transparent 50%, rgba(0,0,0,0.7) 100%);
  z-index: 0;
  pointer-events: none;
}

.content {
  position: relative;
  z-index: 2;
  max-width: 720px;
  margin-bottom: 3.5rem;
  animation: fadeIn 1s ease-out forwards;
}

.content h2 {
  font-size: 2.6rem;
  font-weight: 700;
  letter-spacing: 1px;
  color: #f2858e;
  margin-bottom: 1rem;
  text-shadow: 0 0 15px rgba(242, 133, 142, 0.4);
}

.content p {
  font-size: 1.2rem;
  line-height: 1.8;
  opacity: 0.9;
}

/* 🖼️ Galeri foto sinematik */
.photos {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1.6rem;
  position: relative;
  z-index: 1;
}

.photo {
  width: 290px;
  height: 290px;
  border-radius: 18px;
  object-fit: cover;
  filter: brightness(0.95) contrast(1.1);
  box-shadow: 0 10px 35px rgba(242, 133, 142, 0.25);
  transition: all 0.5s ease;
  cursor: pointer;
}

.photo:hover {
  transform: translateY(-12px) scale(1.05) rotate(2deg);
  box-shadow: 0 20px 60px rgba(242, 133, 142, 0.5);
  filter: brightness(1.1) contrast(1.15);
}

/* 🎞️ Efek grain halus */
.story::after {
  content: "";
  position: absolute;
  inset: 0;
  background-image: url("https://www.transparenttextures.com/patterns/noise-pattern-with-subtle-grain.png");
  opacity: 0.15;
  mix-blend-mode: overlay;
  pointer-events: none;
}

/* Responsif */
@media (max-width: 768px) {
  .photo {
    width: 85%;
    height: 260px;
  }

  .content h2 {
    font-size: 2.1rem;
  }

  .content p {
    font-size: 1rem;
  }
}
</style>