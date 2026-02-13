<script setup>
import { ref, watch, computed, onMounted, onUnmounted } from 'vue'
import { getRandomTwilightQuote, getRandomDramaticFact, getRandomSensualMessage } from '../data/dates.js'

const props = defineProps({
  show: Boolean,
  idea: Object
})

const emit = defineEmits(['close', 'regenerate'])

const twilightQuote = ref(null)
const dramaticFact = ref(null)
const sensualMessage = ref(null)
const showConfetti = ref(false)

// Messages exagérés selon la catégorie
const exaggeratedMessages = {
  default: [
    "✨ Cette date sera LÉGENDAIRE ✨",
    "🔥 Prepare for the most EPIC date ever 🔥",
    "💕 L'amour de votre vie (la date, pas la personne... quoique) 💕",
    "⚡ ATTENTION: Risque élevé de tomber encore plus amoureux ⚡",
    "🌟 Approuvé par Edward Cullen himself 🌟",
    "💖 Tellement romantique que même les vampires pleurent 💖",
  ],
  sensuel: [
    "🔥 Oh là là... Bonne soirée 😏🔥",
    "💋 Ne nous remerciez pas... ou si, en fait 💋",
    "🌡️ Attention: température en hausse 🌡️",
    "😈 Les enfants, au lit ! (vous aussi d'ailleurs) 😈",
    "✨ Ce qui se passe après reste entre vous ✨",
    "🔥 Edward n'a jamais été aussi jaloux 🔥",
  ],
  wtf: [
    "🤪 C'est bizarre mais on adore 🤪",
    "😂 Vous allez vous créer des souvenirs... spéciaux 😂",
    "🎲 Le chaos, c'est romantique aussi! 🎲",
    "🤡 L'amour, c'est faire des trucs bizarres ensemble 🤡",
    "✨ Weird = Wonderful ✨",
  ],
  aventure: [
    "🏔️ L'aventure vous appelle (et l'amour aussi) 🏔️",
    "🐺 Team Jacob approuve cette énergie 🐺",
    "⚡ Adrénaline + Amour = Perfection ⚡",
    "🌲 La nature, c'est le meilleur Tinder 🌲",
  ]
}

const currentExaggeratedMessage = ref('')

const isSensualCategory = computed(() => {
  return props.idea?.category?.toLowerCase().includes('sensuel')
})

const isWtfCategory = computed(() => {
  return props.idea?.category?.toLowerCase().includes('wtf')
})

const isAventureCategory = computed(() => {
  return props.idea?.category?.toLowerCase().includes('aventure')
})

function getRandomMessage() {
  let messages = exaggeratedMessages.default

  if (isSensualCategory.value) {
    messages = exaggeratedMessages.sensuel
  } else if (isWtfCategory.value) {
    messages = exaggeratedMessages.wtf
  } else if (isAventureCategory.value) {
    messages = exaggeratedMessages.aventure
  }

  return messages[Math.floor(Math.random() * messages.length)]
}

function refreshContent() {
  twilightQuote.value = getRandomTwilightQuote()
  dramaticFact.value = getRandomDramaticFact()
  sensualMessage.value = getRandomSensualMessage()
  currentExaggeratedMessage.value = getRandomMessage()
  showConfetti.value = true
  setTimeout(() => {
    showConfetti.value = false
  }, 3000)
}

watch(() => props.show, (newVal) => {
  if (newVal) {
    refreshContent()
  }
})

watch(() => props.idea, () => {
  if (props.show) {
    refreshContent()
  }
})

function handleKeydown(e) {
  if (e.key === 'Escape' && props.show) {
    emit('close')
  }
}

onMounted(() => {
  document.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  document.removeEventListener('keydown', handleKeydown)
})

function handleOverlayClick(e) {
  if (e.target === e.currentTarget) {
    emit('close')
  }
}
</script>

<template>
  <Teleport to="body">
    <Transition name="modal">
      <div v-if="show" class="modal-overlay" @click="handleOverlayClick">
        <!-- Confetti -->
        <div v-if="showConfetti" class="confetti-container">
          <span
            v-for="n in 50"
            :key="n"
            class="confetti"
            :style="{
              left: Math.random() * 100 + '%',
              animationDelay: Math.random() * 0.5 + 's',
              backgroundColor: isSensualCategory
                ? ['#d32f2f', '#ff5252', '#ff8a80', '#e91e63', '#f50057'][Math.floor(Math.random() * 5)]
                : ['#e91e63', '#9c27b0', '#ff6b9d', '#ba68c8', '#ffd54f'][Math.floor(Math.random() * 5)]
            }"
          ></span>
        </div>

        <!-- Cœurs flottants pour sensuel -->
        <div v-if="showConfetti && isSensualCategory" class="hearts-container">
          <span
            v-for="n in 20"
            :key="'heart-' + n"
            class="floating-heart-modal"
            :style="{
              left: Math.random() * 100 + '%',
              animationDelay: Math.random() * 2 + 's',
            }"
          >{{ ['❤️', '🔥', '💋', '😏', '💕'][Math.floor(Math.random() * 5)] }}</span>
        </div>

        <div
          class="modal"
          :class="{ sensual: isSensualCategory, wtf: isWtfCategory }"
          :style="idea ? { '--modal-color': idea.color } : {}"
        >
          <button class="close-btn" @click="emit('close')">×</button>

          <!-- Header décoratif -->
          <div class="modal-header">
            <template v-if="isSensualCategory">
              <span class="deco-fire">🔥</span>
              <span class="deco-heart">💋</span>
              <span class="deco-fire">🔥</span>
            </template>
            <template v-else-if="isWtfCategory">
              <span class="deco-bat">🤪</span>
              <span class="deco-heart">🎲</span>
              <span class="deco-bat">🤪</span>
            </template>
            <template v-else>
              <span class="deco-bat">🦇</span>
              <span class="deco-heart">💕</span>
              <span class="deco-bat flip">🦇</span>
            </template>
          </div>

          <div class="modal-content" v-if="idea">
            <!-- Icône de catégorie -->
            <div class="idea-icon">{{ idea.icon }}</div>

            <!-- Catégorie -->
            <p class="idea-category">{{ idea.category }}</p>

            <!-- L'idée principale -->
            <div class="idea-container" :class="{ sensual: isSensualCategory }">
              <p class="idea-text">{{ idea.idea }}</p>
            </div>

            <!-- Message sensuel spécial -->
            <div v-if="isSensualCategory && sensualMessage" class="sensual-tip">
              <p>{{ sensualMessage }}</p>
            </div>

            <!-- Quote Twilight bonus (pas pour sensuel) -->
            <div v-else-if="twilightQuote" class="twilight-bonus">
              <p class="twilight-quote">"{{ twilightQuote.quote }}"</p>
              <p class="twilight-character">— {{ twilightQuote.character }} {{ twilightQuote.emoji }}</p>
            </div>

            <!-- Fait dramatique -->
            <div class="dramatic-fact" v-if="dramaticFact && !isSensualCategory">
              <p>{{ dramaticFact }}</p>
            </div>

            <!-- Actions -->
            <div class="modal-actions">
              <button class="action-btn regenerate" @click="emit('regenerate')">
                <span class="btn-emoji">🎲</span>
                <span>Autre idée</span>
              </button>
              <button
                class="action-btn accept"
                :class="{ sensual: isSensualCategory }"
                @click="emit('close')"
              >
                <span class="btn-emoji">{{ isSensualCategory ? '😏' : '💖' }}</span>
                <span>{{ isSensualCategory ? 'Oh oui!' : "C'est PARFAIT!" }}</span>
              </button>
            </div>

            <!-- Message exagéré -->
            <p class="exaggerated-message" :class="{ sensual: isSensualCategory }">
              {{ currentExaggeratedMessage }}
            </p>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(10px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 1rem;
}

.modal-enter-active,
.modal-leave-active {
  transition: all 0.4s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal,
.modal-leave-to .modal {
  transform: scale(0.8) translateY(50px);
}

.modal {
  background: linear-gradient(145deg, rgba(30, 30, 50, 0.98), rgba(20, 20, 40, 0.98));
  border: 2px solid var(--modal-color, #e91e63);
  border-radius: 30px;
  padding: 2rem 1.5rem;
  max-width: 400px;
  width: 100%;
  position: relative;
  box-shadow:
    0 25px 80px rgba(0, 0, 0, 0.5),
    0 0 60px color-mix(in srgb, var(--modal-color, #e91e63) 30%, transparent);
  max-height: 90vh;
  overflow-y: auto;
}

.modal.sensual {
  background: linear-gradient(145deg, rgba(50, 20, 20, 0.98), rgba(30, 10, 10, 0.98));
  box-shadow:
    0 25px 80px rgba(0, 0, 0, 0.5),
    0 0 80px rgba(211, 47, 47, 0.3);
}

.modal.wtf {
  background: linear-gradient(145deg, rgba(40, 20, 60, 0.98), rgba(20, 10, 40, 0.98));
}

.close-btn {
  position: absolute;
  top: 0.75rem;
  right: 0.75rem;
  width: 36px;
  height: 36px;
  border: none;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  font-size: 1.5rem;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: rotate(90deg);
}

.modal-header {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 1rem;
}

.deco-bat {
  font-size: 1.5rem;
  animation: batFly 2s ease-in-out infinite;
}

.deco-bat.flip {
  transform: scaleX(-1);
}

.deco-fire {
  font-size: 1.5rem;
  animation: firePulse 0.5s ease-in-out infinite alternate;
}

@keyframes firePulse {
  0% { transform: scale(1) rotate(-5deg); }
  100% { transform: scale(1.2) rotate(5deg); }
}

.deco-heart {
  font-size: 2rem;
  animation: heartBeat 1s ease-in-out infinite;
}

@keyframes batFly {
  0%, 100% { transform: translateY(0) rotate(-10deg); }
  50% { transform: translateY(-10px) rotate(10deg); }
}

@keyframes heartBeat {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}

.modal-content {
  text-align: center;
}

.idea-icon {
  font-size: 4rem;
  animation: bounceIn 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

@keyframes bounceIn {
  0% { transform: scale(0) rotate(-180deg); opacity: 0; }
  60% { transform: scale(1.2) rotate(10deg); }
  100% { transform: scale(1) rotate(0deg); opacity: 1; }
}

.idea-category {
  font-family: 'Cinzel Decorative', serif;
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.6);
  text-transform: uppercase;
  letter-spacing: 3px;
  margin: 0.5rem 0;
}

.idea-container {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 20px;
  padding: 1.25rem;
  margin: 1rem 0;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.idea-container.sensual {
  background: linear-gradient(135deg, rgba(211, 47, 47, 0.15), rgba(255, 82, 82, 0.1));
  border-color: rgba(211, 47, 47, 0.3);
}

.idea-text {
  font-family: 'Satisfy', cursive;
  font-size: 1.4rem;
  line-height: 1.5;
  color: white;
  animation: fadeInUp 0.5s ease 0.2s both;
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.sensual-tip {
  background: linear-gradient(135deg, rgba(211, 47, 47, 0.2), rgba(156, 39, 176, 0.15));
  border-radius: 15px;
  padding: 1rem;
  margin: 1rem 0;
  border: 1px solid rgba(211, 47, 47, 0.3);
  font-size: 0.9rem;
  color: #ffab91;
  font-style: italic;
}

.twilight-bonus {
  background: linear-gradient(135deg, rgba(75, 0, 130, 0.3), rgba(138, 43, 226, 0.2));
  border-radius: 15px;
  padding: 1rem;
  margin: 1rem 0;
  border: 1px solid rgba(138, 43, 226, 0.3);
}

.twilight-quote {
  font-style: italic;
  color: #e1bee7;
  font-size: 0.9rem;
  line-height: 1.4;
}

.twilight-character {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.5);
  margin-top: 0.5rem;
}

.dramatic-fact {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.6);
  font-style: italic;
  padding: 0.75rem;
  background: rgba(255, 255, 255, 0.03);
  border-radius: 10px;
  margin: 0.75rem 0;
}

.modal-actions {
  display: flex;
  gap: 0.75rem;
  margin-top: 1.25rem;
}

.action-btn {
  flex: 1;
  padding: 0.875rem;
  font-family: 'Quicksand', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  border: none;
  border-radius: 15px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  transition: all 0.3s ease;
}

.action-btn .btn-emoji {
  font-size: 1.2rem;
}

.action-btn.regenerate {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.action-btn.regenerate:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
}

.action-btn.accept {
  background: linear-gradient(135deg, #e91e63, #9c27b0);
  color: white;
}

.action-btn.accept.sensual {
  background: linear-gradient(135deg, #d32f2f, #e91e63);
}

.action-btn.accept:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 30px rgba(233, 30, 99, 0.4);
}

.exaggerated-message {
  font-family: 'Satisfy', cursive;
  font-size: 1rem;
  color: #ff6b9d;
  margin-top: 1rem;
  animation: glow 2s ease-in-out infinite;
}

.exaggerated-message.sensual {
  color: #ff5252;
  animation: glowRed 2s ease-in-out infinite;
}

@keyframes glow {
  0%, 100% { text-shadow: 0 0 10px rgba(255, 107, 157, 0.5); }
  50% { text-shadow: 0 0 20px rgba(255, 107, 157, 0.8), 0 0 30px rgba(255, 107, 157, 0.4); }
}

@keyframes glowRed {
  0%, 100% { text-shadow: 0 0 10px rgba(255, 82, 82, 0.5); }
  50% { text-shadow: 0 0 20px rgba(255, 82, 82, 0.8), 0 0 30px rgba(211, 47, 47, 0.4); }
}

/* Confetti */
.confetti-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1001;
  overflow: hidden;
}

.confetti {
  position: absolute;
  width: 10px;
  height: 10px;
  top: -10px;
  animation: confettiFall 3s ease-out forwards;
}

@keyframes confettiFall {
  0% {
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: translateY(100vh) rotate(720deg);
    opacity: 0;
  }
}

/* Floating hearts for sensual */
.hearts-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1001;
  overflow: hidden;
}

.floating-heart-modal {
  position: absolute;
  bottom: -20px;
  font-size: 2rem;
  animation: floatUpHeart 4s ease-out forwards;
}

@keyframes floatUpHeart {
  0% {
    transform: translateY(0) scale(0);
    opacity: 0;
  }
  10% {
    opacity: 1;
    transform: translateY(-10vh) scale(1);
  }
  100% {
    transform: translateY(-110vh) scale(0.5) rotate(20deg);
    opacity: 0;
  }
}

/* Responsive */
@media (max-width: 380px) {
  .modal {
    padding: 1.5rem 1rem;
  }

  .idea-text {
    font-size: 1.2rem;
  }

  .modal-actions {
    flex-direction: column;
  }
}
</style>
