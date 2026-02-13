<script setup>
import { ref, onMounted } from 'vue'

const elements = ref([])

const emojis = ['💕', '💖', '💗', '💝', '✨', '💫', '🌹', '🦇', '🌙', '⭐', '🧛', '💀', '❤️']

onMounted(() => {
  for (let i = 0; i < 20; i++) {
    elements.value.push({
      id: i,
      emoji: emojis[Math.floor(Math.random() * emojis.length)],
      left: Math.random() * 100,
      delay: Math.random() * 15,
      duration: 15 + Math.random() * 10,
      size: 0.8 + Math.random() * 1.2
    })
  }
})
</script>

<template>
  <div class="floating-elements">
    <span
      v-for="el in elements"
      :key="el.id"
      class="floating-element"
      :style="{
        left: el.left + '%',
        animationDelay: el.delay + 's',
        animationDuration: el.duration + 's',
        fontSize: el.size + 'rem'
      }"
    >
      {{ el.emoji }}
    </span>
  </div>
</template>

<style scoped>
.floating-elements {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
  z-index: 0;
}

.floating-element {
  position: absolute;
  opacity: 0.15;
  animation: floatUp linear infinite;
  filter: blur(1px);
}

@keyframes floatUp {
  0% {
    transform: translateY(100vh) rotate(0deg) scale(0.5);
    opacity: 0;
  }
  10% {
    opacity: 0.15;
  }
  90% {
    opacity: 0.15;
  }
  100% {
    transform: translateY(-100px) rotate(720deg) scale(1);
    opacity: 0;
  }
}
</style>
