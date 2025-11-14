<template>
  <transition name="fade-splash">
    <div v-if="!isSiteEntered" class="splash-overlay" @click="enterSite">
      <button class="splash-button">
        klik untuk lihat lebih lanjut
      </button>
    </div>
  </transition>

  <section class="gallery">
    <h2 class="title" ref="titleRef">MOMENTS</h2>

    <div class="grid">
      <div class="row-wrapper">
        <div 
          v-for="(video, i) in portraitVideos" 
          :key="'p-'+i" 
          class="card portrait"
        >
          <div class="glow"></div>
          <div class="grain"></div>
          <video
            :ref="el => { if (el) portraitVideoRefs[i] = el }"
            :src="video.src"
            autoplay
            muted
            loop
            playsinline
            preload="metadata"
            class="video"
          ></video>
          <p class="caption">{{ video.caption }}</p>
        </div>
      </div>

      <div class="row-wrapper">
        <div 
          v-for="(video, i) in landscapeVideos" 
          :key="'l-'+i" 
          class="card landscape"
        >
          <div class="glow"></div>
          <div class="grain"></div>
          <video
            :ref="el => { if (el) landscapeVideoRefs[i] = el }"
            :src="video.src"
            autoplay
            muted
            loop
            playsinline
            preload="metadata"
            class="video"
          ></video>
          <p class="caption">{{ video.caption }}</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import DebugOnly from './DebugOnly.vue'

import { ref, onMounted } from 'vue'

import ch1 from '../assets/story/chery1.mp4'
import ch2 from '../assets/story/chery2.mp4'
import ch3 from '../assets/story/chery3.mp4'
import ch4 from '../assets/story/chery4.mp4'
import ch5 from '../assets/story/chery5.mp4'
import ch1m from '../assets/story/chery1.mov'
import ch2m from '../assets/story/chery2.mov'
import ch3m from '../assets/story/chery3.mov'
import ch4m from '../assets/story/chery4.mp4'
import ch6m from '../assets/story/chery6.mov'

const portraitVideos = [
  { src: ch1, caption: 'The first time I saw your smile' },
  { src: ch2, caption: 'The world turned softer that day' },
  { src: ch1m, caption: 'And now, you live in my silence' },
  { src: ch4m, caption: 'Warmth that stayed even in the dark' },
  { src: ch5, caption: 'A moment frozen in soft red light' },
]
const landscapeVideos = [
  { src: ch2m, caption: 'Your presence, louder than words' },
  { src: ch3m, caption: 'And suddenly everything felt alive' },
  { src: ch6m, caption: 'A moment I never forgot' },
  { src: ch3, caption: 'Every laugh, a memory that stayed' },
]

const portraitVideoRefs = ref([])
const landscapeVideoRefs = ref([])
const isSiteEntered = ref(false)
const titleRef = ref(null)

const playVideo = (video) => {
  if (video && typeof video.play === 'function') {
    const promise = video.play();
    if (promise !== undefined) {
      promise.catch(error => console.error("Video play failed:", error));
    }
  }
};

const activateTitleAnimation = () => {
  const title = titleRef.value
  if (!title) return

  const text = title.textContent
  title.innerHTML = text
    .split('')
    .map(char => char === ' ' ? `&nbsp;` : `<span>${char}</span>`)
    .join('')

  const letters = title.querySelectorAll('span')
  if (letters.length === 0) return 

  const maxPush = 25
  const repelRadius = 100

  title.addEventListener('mousemove', (e) => {
    const rect = title.getBoundingClientRect()
    const mouseX = e.clientX - rect.left
    const mouseY = e.clientY - rect.top

    letters.forEach(letter => {
      const letterX = letter.offsetLeft + letter.offsetWidth / 2
      const letterY = letter.offsetTop + letter.offsetHeight / 2

      const deltaX = mouseX - letterX
      const deltaY = mouseY - letterY
      const distance = Math.sqrt(deltaX * deltaX + deltaY * deltaY)

      if (distance < repelRadius) {
        const force = 1 - (distance / repelRadius)
        const pushX = -(deltaX / distance) * force * maxPush
        const pushY = -(deltaY / distance) * force * maxPush
        
        letter.style.transform = `translate(${pushX}px, ${pushY}px)`
      } else {
        letter.style.transform = 'translate(0, 0)'
      }
    })
  })

  title.addEventListener('mouseleave', () => {
    letters.forEach(letter => {
      letter.style.transform = 'translate(0, 0)'
    })
  })
}

const enterSite = () => {
  isSiteEntered.value = true;
  portraitVideoRefs.value.forEach(playVideo);
  landscapeVideoRefs.value.forEach(playVideo);
  activateTitleAnimation();
};

onMounted(() => {
  // Intentionally left blank
})
</script>

<style scoped>
.splash-overlay {
  position: fixed;
  inset: 0;
  background: #000;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  cursor: pointer;
}

.splash-button {
  font-family: inherit;
  font-size: 1.5rem;
  letter-spacing: 2px;
  color: #f2858e;
  background: transparent;
  border: none;
  padding: 1rem 2.5rem;
  cursor: pointer;
  font-style: italic;
  transition: all 0.3s ease;
  animation: pulse-text 2.5s infinite ease-in-out;
}

@keyframes pulse-text {
  0%, 100% {
    opacity: 0.6;
    text-shadow: 0 0 12px rgba(242, 133, 142, 0.4);
  }
  50% {
    opacity: 1;
    text-shadow: 0 0 20px rgba(242, 133, 142, 0.8);
  }
}

.fade-splash-leave-active {
  transition: opacity 0.5s ease;
}
.fade-splash-leave-to {
  opacity: 0;
}

.gallery {
  padding: 5rem 2rem;
  background: radial-gradient(circle at top, #000 0%, #0a0608 80%);
  text-align: center;
  color: #fff;
  overflow: hidden; 
  max-width: 100%; 
}

.title {
  position: relative;
  font-size: 2.7rem;
  letter-spacing: 4px;
  color: #f2858e;
  margin-bottom: 3rem;
  text-shadow:
    0 0 12px rgba(242,133,142,0.6),
    0 0 20px rgba(122,34,70,0.5);
  padding: 1rem;
  cursor: default;
}

.title :deep(span) {
  display: inline-block;
  transition: transform 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.grid {
  display: flex;
  flex-direction: column;
  gap: 2.4rem; 
}

.row-wrapper {
  display: flex;
  gap: 1.5rem; 
}

.card {
  position: relative;
  border-radius: 22px;
  overflow: hidden;
  background: #000;
  display: flex;
  align-items: flex-end;
  padding-bottom: 12px;
  transition: 0.6s ease;
  flex: 1;
  min-width: 0; 
}

.portrait {
  aspect-ratio: 9 / 16; 
}

.landscape {
  aspect-ratio: 16 / 9; 
}

.card:hover {
  transform: translateY(-8px) scale(1.025);
}

.video {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover; 
  background: #000;
  filter: brightness(0.9) contrast(1.08);
  transition: 0.5s ease;
  pointer-events: none; 
}

.card:hover .video {
  filter: brightness(1) contrast(1.12);
}

.glow {
  position: absolute;
  inset: 0;
  pointer-events: none;
  background: radial-gradient(
    circle at center,
    rgba(255,60,80,0.07) 0%,
    rgba(0,0,0,0.9) 70%
  );
}

.grain {
  position: absolute;
  inset: 0;
  opacity: 0.08;
  pointer-events: none;
  background-image: url("data:image/svg+xml;utf8,\<svg xmlns='http://www.w3.org/2000/svg' width='160' height='160'>\<filter id='grain'>\<feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='1'/>\</filter>\<rect width='100%' height='100%' filter='url(%23grain)'/>\</svg>");
}

.caption {
  position: relative;
  z-index: 4;
  width: 100%;
  text-align: center;
  background: rgba(0,0,0,0.55);
  border-radius: 12px;
  padding: 5px 10px;
  margin: 8px auto;
  font-size: 0.9rem;
  font-style: italic;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

@media (max-width: 600px) {
  .gallery { 
    padding: 3rem 1rem; 
  }
  .row-wrapper { 
    gap: 0.75rem; 
  }
  .card { 
    border-radius: 14px; 
  }
  .caption { 
    font-size: 0.75rem; 
    padding: 3px 6px;
  }
  .splash-button {
    font-size: 1rem;
  }
  .title {
    font-size: 2rem;
  }
}
</style>