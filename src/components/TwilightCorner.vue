<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { getRandomTwilightQuote } from '../data/dates.js'

const isExpanded = ref(false)
const currentQuote = ref(null)

let interval = null

function toggleExpand() {
  isExpanded.value = !isExpanded.value
  if (isExpanded.value) {
    currentQuote.value = getRandomTwilightQuote()
  }
}

function newQuote() {
  currentQuote.value = getRandomTwilightQuote()
}

onMounted(() => {
  currentQuote.value = getRandomTwilightQuote()
})
</script>

<template>
  <div class="twilight-corner" :class="{ expanded: isExpanded }">
    <button class="twilight-toggle" @click="toggleExpand">
      <span class="toggle-icon">🧛</span>
      <span class="toggle-text" v-if="!isExpanded">Team Edward?</span>
      <span class="toggle-close" v-else>×</span>
    </button>

    <Transition name="slide">
      <div v-if="isExpanded" class="twilight-content">
        <h4 class="twilight-title">
          <span>🌙</span> Zone Twilight <span>🌙</span>
        </h4>

        <div class="twilight-quote-box" v-if="currentQuote">
          <p class="quote-text">"{{ currentQuote.quote }}"</p>
          <p class="quote-author">— {{ currentQuote.character }}</p>
          <span class="quote-emoji">{{ currentQuote.emoji }}</span>
        </div>

        <button class="new-quote-btn" @click="newQuote">
          🔮 Nouvelle prophétie
        </button>

        <div class="twilight-facts">
          <p class="fact-title">📚 Le saviez-vous?</p>
          <p class="fact-text">Edward a 104 ans de différence avec Bella. L'amour n'a pas d'âge... littéralement.</p>
        </div>

        <div class="team-selector">
          <span class="team team-edward">🧛 Team Edward</span>
          <span class="vs">VS</span>
          <span class="team team-jacob">🐺 Team Jacob</span>
        </div>

        <p class="disclaimer">
          (On ne juge pas, mais Edward scintille au soleil, donc...)
        </p>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
.twilight-corner {
  position: relative;
  z-index: 10;
  margin: 1rem 0;
}

.twilight-toggle {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: linear-gradient(135deg, rgba(75, 0, 130, 0.4), rgba(138, 43, 226, 0.3));
  border: 1px solid rgba(138, 43, 226, 0.4);
  border-radius: 25px;
  padding: 0.5rem 1rem;
  color: #e1bee7;
  cursor: pointer;
  transition: all 0.3s ease;
  font-family: 'Quicksand', sans-serif;
  font-size: 0.9rem;
}

.twilight-toggle:hover {
  background: linear-gradient(135deg, rgba(75, 0, 130, 0.6), rgba(138, 43, 226, 0.5));
  transform: scale(1.05);
  box-shadow: 0 0 20px rgba(138, 43, 226, 0.3);
}

.toggle-icon {
  font-size: 1.3rem;
  animation: float 2s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-3px); }
}

.toggle-close {
  font-size: 1.2rem;
  font-weight: bold;
}

.twilight-content {
  background: linear-gradient(145deg, rgba(30, 0, 50, 0.95), rgba(20, 0, 40, 0.98));
  border: 1px solid rgba(138, 43, 226, 0.3);
  border-radius: 20px;
  padding: 1.25rem;
  margin-top: 0.75rem;
  text-align: center;
}

.slide-enter-active,
.slide-leave-active {
  transition: all 0.3s ease;
}

.slide-enter-from,
.slide-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

.twilight-title {
  font-family: 'Cinzel Decorative', serif;
  font-size: 1rem;
  color: #ce93d8;
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.twilight-quote-box {
  background: rgba(138, 43, 226, 0.15);
  border-radius: 15px;
  padding: 1rem;
  margin-bottom: 1rem;
}

.quote-text {
  font-style: italic;
  color: #e1bee7;
  font-size: 0.95rem;
  line-height: 1.5;
}

.quote-author {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.5);
  margin-top: 0.5rem;
}

.quote-emoji {
  font-size: 1.5rem;
  display: block;
  margin-top: 0.5rem;
}

.new-quote-btn {
  background: rgba(138, 43, 226, 0.3);
  border: 1px solid rgba(138, 43, 226, 0.4);
  border-radius: 20px;
  padding: 0.5rem 1rem;
  color: #e1bee7;
  cursor: pointer;
  font-family: 'Quicksand', sans-serif;
  font-size: 0.85rem;
  transition: all 0.3s ease;
  margin-bottom: 1rem;
}

.new-quote-btn:hover {
  background: rgba(138, 43, 226, 0.5);
  transform: scale(1.05);
}

.twilight-facts {
  background: rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  padding: 0.75rem;
  margin-bottom: 1rem;
}

.fact-title {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.6);
  margin-bottom: 0.25rem;
}

.fact-text {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.4;
}

.team-selector {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  margin-bottom: 0.75rem;
}

.team {
  font-size: 0.8rem;
  padding: 0.4rem 0.75rem;
  border-radius: 15px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.team-edward {
  background: rgba(100, 100, 150, 0.3);
  border: 1px solid rgba(150, 150, 200, 0.3);
  color: #b0b0d0;
}

.team-jacob {
  background: rgba(139, 90, 43, 0.3);
  border: 1px solid rgba(180, 120, 60, 0.3);
  color: #d4a574;
}

.team:hover {
  transform: scale(1.1);
}

.vs {
  font-size: 0.7rem;
  color: rgba(255, 255, 255, 0.4);
  font-weight: bold;
}

.disclaimer {
  font-size: 0.7rem;
  color: rgba(255, 255, 255, 0.4);
  font-style: italic;
}
</style>
