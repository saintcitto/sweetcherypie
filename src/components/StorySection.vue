<template>
  <section ref="section" class="story">
    <div class="red-haze"></div>
    <div class="grain"></div>
    <div class="scan"></div>

    <div class="content">
      <h2 class="title">My Pride</h2>
      <p class="subtitle">
        Every heartbeat and every silence had its own meaning.<br />
        These moments remind me of how love taught me peace again.
      </p>
    </div>

    <div class="photos">
      <img src="../assets/story/chery1.jpeg" class="photo" />
      <img src="../assets/story/chery2.jpeg" class="photo" />
      <img src="../assets/story/chery3.jpeg" class="photo" />
      <img src="../assets/story/chery4.jpeg" class="photo" />
      <img src="../assets/story/chery5.jpeg" class="photo" />
    </div>
  </section>
</template>

<script setup>
import DebugOnly from './DebugOnly.vue'
</script>

<style scoped>
.story {
  position: relative;
  padding: 6rem 2rem;
  background: radial-gradient(circle at center, #000 40%, #0d0507 100%);
  color: #fff;
  text-align: center;
  overflow: hidden;
}

.red-haze { 
  position: absolute; 
  inset: 0; 
  pointer-events: none; 
  z-index: 1; 
  background: radial-gradient(circle at center, rgba(255, 0, 60, 0.28) 0%, rgba(0, 0, 0, 0.92) 70%); 
}
.grain { 
  position: absolute; 
  inset: 0; 
  pointer-events: none; 
  z-index: 2; 
  opacity: 0.18; 
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='200' height='200'><filter id='grain'><feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2'/></filter><rect width='100%' height='100%' filter='url(%23grain)' opacity='0.35'/></svg>"); 
}
.scan { 
  position: absolute; 
  inset: 0; 
  pointer-events: none; 
  z-index: 3; 
  opacity: 0.12; 
  background: repeating-linear-gradient(transparent 0px, transparent 2px, rgba(255, 255, 255, 0.05) 4px, transparent 6px); 
}

.content {
  position: relative;
  z-index: 10;
  max-width: 750px;
  margin: 0 auto 3.8rem;
}

.title {
  font-size: 2.6rem;
  font-weight: 700;
  letter-spacing: 3px;
  color: #f2858e;
  text-shadow:
    0 0 18px rgba(242, 133, 142, 1),
    0 0 40px rgba(122, 34, 70, 0.8);
}

.subtitle {
  margin-top: 1rem;
  opacity: 0.85;
  line-height: 1.6;
  font-size: 1.15rem;
}

.photos {
  position: relative;
  z-index: 12;
  display: flex;
  justify-content: center;
  gap: 1.4rem;
  flex-wrap: wrap;
}

.photo {
  width: 260px;
  height: 340px;
  border-radius: 20px;
  object-fit: cover;
  filter: brightness(0.92) contrast(1.1);
  box-shadow: 0 0 30px rgba(242, 133, 142, 0.35);
  transition: 0.4s ease;
}

.photo:hover {
  transform: translateY(-12px) scale(1.04);
  filter: brightness(1) contrast(1.18);
}
</style>