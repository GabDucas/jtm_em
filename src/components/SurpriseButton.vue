<script setup>
import { ref } from 'vue'

const emit = defineEmits(['click'])
const isPressed = ref(false)

function handleClick() {
  isPressed.value = true
  setTimeout(() => {
    isPressed.value = false
  }, 300)
  emit('click')
}
</script>

<template>
  <div class="surprise-container">
    <div class="dramatic-text">
      <span class="arrow">↓</span>
      Ou laisse le destin (et les vampires) décider
      <span class="arrow">↓</span>
    </div>

    <button
      class="surprise-btn"
      :class="{ pressed: isPressed }"
      @click="handleClick"
    >
      <span class="btn-bg"></span>
      <span class="btn-content">
        <span class="btn-icon">🎲</span>
        <span class="btn-text">SURPRISE TOTALE</span>
        <span class="btn-subtext">« Comme le destin de Bella »</span>
      </span>
      <span class="btn-sparkle btn-sparkle-1">✨</span>
      <span class="btn-sparkle btn-sparkle-2">💫</span>
      <span class="btn-sparkle btn-sparkle-3">⭐</span>
    </button>

    <p class="warning-text">
      ⚠️ Attention : peut contenir des doses excessives d'amour ⚠️
    </p>
  </div>
</template>

<style scoped>
.surprise-container {
  padding: 1.5rem 0;
  position: relative;
  z-index: 1;
  text-align: center;
}

.dramatic-text {
  font-family: 'Satisfy', cursive;
  font-size: 1rem;
  color: rgba(255, 255, 255, 0.6);
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.arrow {
  animation: bounce 1s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(5px); }
}

.surprise-btn {
  width: 100%;
  padding: 1.5rem 2rem;
  font-family: 'Quicksand', sans-serif;
  font-size: 1.1rem;
  font-weight: 700;
  color: white;
  background: linear-gradient(135deg, #e91e63, #9c27b0);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow:
    0 10px 30px rgba(233, 30, 99, 0.4),
    0 0 0 0 rgba(233, 30, 99, 0.4);
}

.btn-bg {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, #ff6b9d, #ba68c8);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.surprise-btn:hover .btn-bg {
  opacity: 1;
}

.surprise-btn:hover {
  transform: translateY(-5px) scale(1.02);
  box-shadow:
    0 20px 50px rgba(233, 30, 99, 0.5),
    0 0 0 10px rgba(233, 30, 99, 0.1),
    0 0 60px rgba(156, 39, 176, 0.3);
}

.surprise-btn.pressed {
  transform: translateY(0) scale(0.95);
}

.btn-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
}

.btn-icon {
  font-size: 2rem;
  animation: spin 3s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.btn-text {
  letter-spacing: 2px;
}

.btn-subtext {
  font-family: 'Satisfy', cursive;
  font-size: 0.9rem;
  font-weight: 400;
  opacity: 0.8;
}

.btn-sparkle {
  position: absolute;
  font-size: 1.5rem;
  opacity: 0;
  transition: all 0.3s ease;
  pointer-events: none;
}

.surprise-btn:hover .btn-sparkle {
  opacity: 1;
}

.btn-sparkle-1 {
  top: -10px;
  left: 20%;
  animation: floatSparkle 2s ease-in-out infinite;
}

.btn-sparkle-2 {
  top: -5px;
  right: 15%;
  animation: floatSparkle 2s ease-in-out infinite 0.5s;
}

.btn-sparkle-3 {
  bottom: -10px;
  left: 50%;
  animation: floatSparkle 2s ease-in-out infinite 1s;
}

@keyframes floatSparkle {
  0%, 100% {
    transform: translateY(0) rotate(0deg);
    opacity: 0.5;
  }
  50% {
    transform: translateY(-10px) rotate(180deg);
    opacity: 1;
  }
}

.warning-text {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.4);
  margin-top: 0.75rem;
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 0.4; }
  50% { opacity: 0.7; }
}
</style>
