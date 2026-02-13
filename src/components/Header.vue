<script setup>
import { ref, onMounted } from 'vue'

const sparkleCount = ref(0)

onMounted(() => {
  setInterval(() => {
    sparkleCount.value++
  }, 2000)
})
</script>

<template>
  <header class="header">
    <div class="logo-container">
      <span class="vampire-fang">🧛</span>
      <div class="title-wrapper">
        <h1 class="title">
          <span class="title-heart">💕</span>
          Date Night
          <span class="title-heart">💕</span>
        </h1>
        <div class="subtitle-container">
          <span class="sparkle">✨</span>
          <span class="subtitle">Twilight Edition</span>
          <span class="sparkle">✨</span>
        </div>
      </div>
      <span class="vampire-fang flip">🧛</span>
    </div>

    <p class="tagline">
      🌙 Pour un amour aussi <span class="dramatic">INTENSE</span> et
      <span class="dramatic">ÉTERNEL</span> que celui d'Edward & Bella 🌙
    </p>

    <div class="decorative-line">
      <span>💀</span>
      <span class="line"></span>
      <span>❤️</span>
      <span class="line"></span>
      <span>💀</span>
    </div>
  </header>
</template>

<style scoped>
.header {
  text-align: center;
  padding: 1.5rem 0;
  position: relative;
  z-index: 1;
}

.logo-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.vampire-fang {
  font-size: 2rem;
  animation: float 3s ease-in-out infinite;
}

.vampire-fang.flip {
  transform: scaleX(-1);
  animation: float 3s ease-in-out infinite reverse;
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

.vampire-fang.flip {
  animation-name: floatFlip;
}

@keyframes floatFlip {
  0%, 100% { transform: scaleX(-1) translateY(0); }
  50% { transform: scaleX(-1) translateY(-10px); }
}

.title-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.title {
  font-family: 'Cinzel Decorative', serif;
  font-size: 2rem;
  font-weight: 700;
  background: linear-gradient(135deg, #fff, #ffb6c1, #fff);
  background-size: 200% 200%;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: shimmer 3s ease-in-out infinite;
  text-shadow: 0 0 30px rgba(233, 30, 99, 0.5);
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

@keyframes shimmer {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.title-heart {
  font-size: 1.5rem;
  animation: heartPulse 1s ease-in-out infinite;
}

@keyframes heartPulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.3); }
}

.subtitle-container {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-top: 0.25rem;
}

.subtitle {
  font-family: 'Satisfy', cursive;
  font-size: 1.2rem;
  color: #b39ddb;
  letter-spacing: 2px;
}

.sparkle {
  animation: sparkle 1.5s ease-in-out infinite;
}

@keyframes sparkle {
  0%, 100% { opacity: 1; transform: scale(1) rotate(0deg); }
  50% { opacity: 0.5; transform: scale(1.3) rotate(180deg); }
}

.tagline {
  color: rgba(255, 255, 255, 0.8);
  font-size: 0.9rem;
  font-weight: 400;
  margin-top: 0.5rem;
  line-height: 1.6;
}

.dramatic {
  font-weight: 700;
  color: #ff6b9d;
  text-transform: uppercase;
  animation: dramaticPulse 2s ease-in-out infinite;
}

@keyframes dramaticPulse {
  0%, 100% { color: #ff6b9d; }
  50% { color: #e91e63; text-shadow: 0 0 10px #e91e63; }
}

.decorative-line {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin-top: 1rem;
  opacity: 0.6;
}

.line {
  width: 50px;
  height: 1px;
  background: linear-gradient(90deg, transparent, #ff6b9d, transparent);
}
</style>
