<template>
  <section ref="section" class="gallery">
    <h2>Moments</h2>

    <div class="story-container">
      <div
        v-for="(video, index) in videos"
        :key="index"
        class="story"
        @mouseenter="pauseVideo(index)"
        @mouseleave="playVideo(index)"
      >
        <video
          ref="storyVideos"
          :src="video.src"
          autoplay
          muted
          loop
          playsinline
        ></video>

        <div class="story-overlay"></div>

        <div class="caption">
          <span class="caption-text">{{ video.caption }}</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue'
import { gsap } from 'gsap'

// ✅ Import video agar dikenali Vite (bukan pakai path /src)
import chery1 from '../assets/story/chery1.mp4'
import chery2 from '../assets/story/chery2.mp4'
import chery3 from '../assets/story/chery3.mp4'
import chery4 from '../assets/story/chery4.mp4'

const storyVideos = ref([])

const videos = [
  { src: chery1, caption: 'The first time I saw your smile' },
  { src: chery2, caption: 'The world turned softer that day' },
  { src: chery3, caption: 'Every laugh, a memory that stayed' },
  { src: chery4, caption: 'And now, you live in my silence' }
]

onMounted(async () => {
  await nextTick()

  // Fade-in lembut & floating motion biar harmonis sama music ambience
  gsap.from('.story', {
    opacity: 0,
    y: 60,
    duration: 2.5,
    stagger: 0.4,
    ease: 'power3.out'
  })

  // Floating motion lembut (naik turun pelan)
  gsap.to('.story', {
    y: 20,
    duration: 6,
    repeat: -1,
    yoyo: true,
    ease: 'sine.inOut'
  })

  // Caption fade-in dengan ritme lembut
  gsap.from('.caption-text', {
    opacity: 0,
    y: 25,
    duration: 2.8,
    delay: 0.8,
    stagger: 0.5,
    ease: 'slow.out'
  })

  // Pastikan semua video play bareng
  storyVideos.value.forEach((vid) => {
    vid.play().catch(() => {
      console.warn('Autoplay mungkin diblokir browser, user interaction dibutuhkan.')
    })
  })
})

const pauseVideo = (index) => {
  storyVideos.value[index]?.pause()
}

const playVideo = (index) => {
  storyVideos.value[index]?.play()
}
</script>

<style scoped>
.gallery {
  background: radial-gradient(circle at center, #000 40%, #111 100%);
  text-align: center;
  padding: 5rem 2rem;
  color: #fff;
  position: relative;
  overflow: hidden;
}

.gallery h2 {
  font-size: 2.2rem;
  font-weight: 600;
  letter-spacing: 1.5px;
  margin-bottom: 2rem;
  text-transform: uppercase;
  color: #f2858e;
  text-shadow: 0 0 10px rgba(242, 133, 142, 0.4);
}

.story-container {
  display: flex;
  justify-content: center;
  gap: 2rem;
  flex-wrap: wrap;
}

.story {
  position: relative;
  width: 200px;
  height: 350px;
  border-radius: 24px;
  overflow: hidden;
  box-shadow: 0 0 25px rgba(242, 133, 142, 0.18);
  cursor: pointer;
  transition: transform 0.8s ease, box-shadow 0.8s ease;
  background: #000;
}

.story:hover {
  transform: scale(1.06);
  box-shadow: 0 0 40px rgba(242, 133, 142, 0.35);
}

.story video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 24px;
  filter: brightness(0.85) contrast(1.05) saturate(1.1);
  transition: filter 0.6s ease;
}

.story:hover video {
  filter: brightness(1) contrast(1.15) saturate(1.2);
}

.story-overlay {
  position: absolute;
  inset: 0;
  border: 1.5px solid rgba(242, 133, 142, 0.35);
  border-radius: 24px;
  pointer-events: none;
  animation: borderGlow 5s infinite ease-in-out;
}

.caption {
  position: absolute;
  bottom: 18px;
  width: 100%;
  text-align: center;
  padding: 0 10px;
}

.caption-text {
  display: inline-block;
  background: rgba(0, 0, 0, 0.55);
  padding: 6px 14px;
  border-radius: 18px;
  font-size: 0.95rem;
  font-weight: 300;
  color: #fff;
  letter-spacing: 0.5px;
  font-style: italic;
  animation: textPulse 6s ease-in-out infinite alternate;
}

@keyframes borderGlow {
  0%, 100% {
    box-shadow: 0 0 8px rgba(242, 133, 142, 0.35);
  }
  50% {
    box-shadow: 0 0 20px rgba(242, 133, 142, 0.7);
  }
}

@keyframes textPulse {
  0% {
    text-shadow: 0 0 5px #f2858e, 0 0 10px #f2858e;
  }
  100% {
    text-shadow: 0 0 14px #f2858e, 0 0 26px #f2858e;
  }
}
</style>