<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { getRandomCheesyQuote, getRandomTwilightQuote } from '../data/dates.js'

const currentQuote = ref(null)
const isTwilight = ref(false)
const isAnimating = ref(false)

let interval = null

function changeQuote() {
  isAnimating.value = true

  setTimeout(() => {
    // Alterner entre quotes ringardes et Twilight
    isTwilight.value = Math.random() > 0.6

    if (isTwilight.value) {
      const twilight = getRandomTwilightQuote()
      currentQuote.value = {
        text: `"${twilight.quote}"`,
        author: `— ${twilight.character} 🧛`,
        emoji: twilight.emoji
      }
    } else {
      const cheesy = getRandomCheesyQuote()
      currentQuote.value = {
        text: cheesy.quote,
        author: null,
        emoji: cheesy.emoji
      }
    }

    isAnimating.value = false
  }, 300)
}

onMounted(() => {
  changeQuote()
  interval = setInterval(changeQuote, 8000)
})

onUnmounted(() => {
  if (interval) clearInterval(interval)
})
</script>

<template>
  <div class="quote-banner" :class="{ twilight: isTwilight, animating: isAnimating }">
    <div class="quote-decoration left">{{ isTwilight ? '🌹' : '💝' }}</div>

    <div class="quote-content" v-if="currentQuote">
      <p class="quote-text">{{ currentQuote.text }}</p>
      <p class="quote-author" v-if="currentQuote.author">{{ currentQuote.author }}</p>
      <span class="quote-emoji">{{ currentQuote.emoji }}</span>
    </div>

    <div class="quote-decoration right">{{ isTwilight ? '🌹' : '💝' }}</div>

    <button class="refresh-quote" @click="changeQuote" title="Nouvelle citation">
      🔄
    </button>
  </div>
</template>

<style scoped>
.quote-banner {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  padding: 1rem;
  margin: 1rem 0;
  position: relative;
  overflow: hidden;
  backdrop-filter: blur(10px);
  transition: all 0.5s ease;
}

.quote-banner.twilight {
  background: rgba(75, 0, 130, 0.2);
  border-color: rgba(138, 43, 226, 0.3);
  box-shadow: 0 0 20px rgba(138, 43, 226, 0.2);
}

.quote-banner.animating .quote-content {
  opacity: 0;
  transform: translateY(-10px);
}

.quote-content {
  text-align: center;
  transition: all 0.3s ease;
  opacity: 1;
  transform: translateY(0);
}

.quote-text {
  font-family: 'Satisfy', cursive;
  font-size: 1.1rem;
  color: white;
  line-height: 1.5;
  margin-bottom: 0.5rem;
}

.twilight .quote-text {
  font-style: italic;
  color: #e1bee7;
}

.quote-author {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.6);
  font-weight: 500;
}

.quote-emoji {
  font-size: 1.5rem;
  display: block;
  margin-top: 0.5rem;
  animation: bounce 2s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-5px); }
}

.quote-decoration {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  font-size: 1.2rem;
  opacity: 0.5;
  animation: sway 3s ease-in-out infinite;
}

.quote-decoration.left {
  left: 0.5rem;
}

.quote-decoration.right {
  right: 0.5rem;
  animation-delay: 1.5s;
}

@keyframes sway {
  0%, 100% { transform: translateY(-50%) rotate(-5deg); }
  50% { transform: translateY(-50%) rotate(5deg); }
}

.refresh-quote {
  position: absolute;
  bottom: 0.25rem;
  right: 0.25rem;
  background: none;
  border: none;
  font-size: 0.8rem;
  cursor: pointer;
  opacity: 0.4;
  transition: all 0.3s ease;
  padding: 0.25rem;
}

.refresh-quote:hover {
  opacity: 1;
  transform: rotate(180deg);
}
</style>
