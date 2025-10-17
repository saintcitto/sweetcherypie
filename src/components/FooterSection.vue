<template>
  <footer class="footer">
    <div class="footer-content">
      <span class="heart">❤️</span>
      <p class="footer-text">© 2025 <strong>For Chery</strong> — Written with Love by Saint Vimalacitto.</p>
    </div>
    <p class="subtext">Every line, a memory that still breathes.</p>
  </footer>
</template>

<script setup>
import { onMounted } from 'vue'
import { gsap } from 'gsap'

onMounted(() => {
  gsap.from('.footer-content', {
    opacity: 0,
    y: 30,
    duration: 1.2,
    ease: 'power3.out'
  })
  gsap.from('.subtext', {
    opacity: 0,
    y: 20,
    delay: 0.8,
    duration: 1.4,
    ease: 'power2.out'
  })
})
</script>

<style scoped>
.footer {
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, #000 100%);
  padding: 2rem 1rem;
  text-align: center;
  color: #fff;
  font-family: 'Poppins', sans-serif;
  letter-spacing: 0.5px;
  position: relative;
  overflow: hidden;
}

.footer-content {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.4rem;
  margin-bottom: 0.4rem;
}

.heart {
  display: inline-block;
  animation: heartbeat 1.8s infinite ease-in-out;
  color: #f2858e;
  filter: drop-shadow(0 0 6px rgba(242, 133, 142, 0.7));
}

.footer-text {
  font-size: 0.95rem;
  opacity: 0.85;
}

.footer-text strong {
  color: #f2858e;
  font-weight: 600;
}

.subtext {
  font-size: 0.8rem;
  opacity: 0.65;
  font-style: italic;
  animation: glowText 3s infinite alternate;
}

@keyframes heartbeat {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.25);
  }
}

@keyframes glowText {
  from {
    text-shadow: 0 0 6px rgba(242, 133, 142, 0.5);
  }
  to {
    text-shadow: 0 0 12px rgba(242, 133, 142, 0.9);
  }
}
</style>