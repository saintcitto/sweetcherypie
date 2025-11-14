<template>
  <footer class="footer">
    <div class="footer-inner">
      <div class="footer-content">
        <span class="heart">❤️</span>
        <p class="footer-text">
          © 2025 <strong>For Chery</strong> — Written with Love by Saint Vimalacitto.
        </p>
      </div>
      <p class="subtext">Every line, a memory that still breathes.</p>
    </div>

    <div class="grain"></div>
  </footer>
</template>

<script setup>
import { onMounted } from 'vue'
import gsap from 'gsap'

onMounted(() => {
  gsap.from('.footer-inner', {
    opacity: 0,
    y: 32,
    duration: 1.4,
    ease: 'power3.out'
  })

  gsap.to('.footer-inner', {
    y: -4,
    repeat: -1,
    yoyo: true,
    duration: 3.8,
    ease: 'sine.inOut'
  })
})
</script>

<style scoped>
.footer {
  position: relative;
  padding: 3rem 1.4rem 2rem;
  color: #fff;
  text-align: center;
  background: linear-gradient(
      180deg,
      rgba(0, 0, 0, 0) 0%,
      rgba(0, 0, 0, 0.65) 40%,
      #000 100%
  );
  overflow: hidden;
}

.footer-inner {
  position: relative;
  z-index: 3;
}

.footer-content {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.45rem;
  margin-bottom: 0.4rem;
}

.heart {
  color: #f2858e;
  filter: drop-shadow(0 0 10px rgba(242, 133, 142, 0.9));
  animation: heartbeat 2s infinite ease-in-out;
}

.footer-text {
  font-size: 0.95rem;
  opacity: 0.87;
  letter-spacing: 0.3px;
}

.footer-text strong {
  color: #f2858e;
}

.subtext {
  font-size: 0.82rem;
  opacity: 0.6;
  font-style: italic;
  animation: glowText 4s infinite alternate;
}

/* CLEAN INTERNAL GRAIN */
.grain {
  pointer-events: none;
  position: absolute;
  inset: 0;
  z-index: 2;
  opacity: 0.12;
  mix-blend-mode: overlay;

  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='160' height='160'><filter id='grain'><feTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='2'/></filter><rect width='100%' height='100%' filter='url(%23grain)' opacity='0.38'/></svg>");
}

/* ANIMATIONS */
@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.28); }
}

@keyframes glowText {
  from { text-shadow: 0 0 6px rgba(242,133,142,0.45); }
  to { text-shadow: 0 0 16px rgba(242,133,142,0.9); }
}

/* MOBILE */
@media (max-width: 480px) {
  .footer {
    padding: 2.6rem 1rem 1.8rem;
  }

  .footer-text {
    font-size: 0.88rem;
  }

  .subtext {
    font-size: 0.75rem;
  }
}
</style>
