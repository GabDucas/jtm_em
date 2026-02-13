<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const loveMessages = [
  "Je t'aime plus que les vampires aiment le sang 🧛❤️",
  "Tu es ma Bella et je suis ton Edward ✨",
  "Notre amour > l'immortalité 💀💕",
  "Forever and always, mon amour éternel ♾️",
  "Tu fais battre mon cœur mort 💓",
  "Ensemble pour l'éternité (et au-delà) 🌙",
  "Si tu étais un vampire, je me ferais mordre volontairement 🧛💋",
  "Mon amour pour toi est plus profond que les yeux d'Edward 👀✨",
  "Je t'aime à la folie (genre vraiment, c'est un problème) 🤪❤️",
  "Tu es le scintillement de ma peau de vampire ✨🌞",
  "Notre amour: 100% bio, 0% raisonnable 🌱💕",
  "Team 'Nous' pour toujours 🏆❤️",
  "Si l'amour était un sport olympique, on aurait l'or 🥇💕",
  "Je t'aime plus que le wifi gratuit 📶💖",
  "Tu es ma personne préférée (après moi, évidemment) 😏❤️",
  "Mon amour pour toi = ∞ (et je suis nul en maths) 🔢💕",
  "Tu es le 'mot de passe sauvegardé' de mon cœur 🔐",
  "Sans toi, je suis comme Forks sans pluie ☔💔",
  "Je ferais n'importe quoi pour toi (sauf regarder un autre film) 🎬❤️",
  "Tu es le bug que je ne veux jamais corriger 🐛💕",
]

const currentMessage = ref(loveMessages[0])
let interval = null

onMounted(() => {
  // Start with random message
  currentMessage.value = loveMessages[Math.floor(Math.random() * loveMessages.length)]

  interval = setInterval(() => {
    // Random message each time
    const newIndex = Math.floor(Math.random() * loveMessages.length)
    currentMessage.value = loveMessages[newIndex]
  }, 5000)
})

onUnmounted(() => {
  if (interval) clearInterval(interval)
})
</script>

<template>
  <footer class="footer">
    <div class="footer-decoration">
      <span class="bat-left">🦇</span>
      <span class="line"></span>
      <span class="heart-container">
        <span class="heart">❤️</span>
      </span>
      <span class="line"></span>
      <span class="bat-right">🦇</span>
    </div>

    <p class="love-message">{{ currentMessage }}</p>

    <div class="fun-stats">
      <span class="stat">
        <span class="stat-number">∞</span>
        <span class="stat-label">idées de dates</span>
      </span>
      <span class="stat-divider">•</span>
      <span class="stat">
        <span class="stat-number">147+</span>
        <span class="stat-label">possibilités</span>
      </span>
      <span class="stat-divider">•</span>
      <span class="stat">
        <span class="stat-number">1</span>
        <span class="stat-label">amour éternel</span>
      </span>
    </div>

    <p class="credits">
      Fait avec <span class="beating-heart">❤️</span> et une quantité déraisonnable de romantisme
    </p>

    <p class="twilight-disclaimer">
      🧛 Aucun vampire n'a été blessé dans la création de cette app 🧛
    </p>

    <p class="copyright">
      © {{ new Date().getFullYear() }} - Pour nous deux, pour toujours 💕
    </p>

    <div class="sparkle-footer">
      <span v-for="n in 7" :key="n" class="footer-sparkle">
        {{ ['✨', '💫', '⭐', '🌟', '💖', '🦇', '🌙'][n-1] }}
      </span>
    </div>

    <p class="easter-egg">
      Psst... Tu es magnifique aujourd'hui 💕
    </p>
  </footer>
</template>

<style scoped>
.footer {
  text-align: center;
  padding: 2rem 0 1rem;
  position: relative;
  z-index: 1;
  margin-top: 1rem;
}

.footer-decoration {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
  opacity: 0.6;
}

.bat-left, .bat-right {
  animation: batFly 3s ease-in-out infinite;
}

.bat-right {
  animation-delay: 1.5s;
  transform: scaleX(-1);
}

@keyframes batFly {
  0%, 100% { transform: translateY(0) rotate(-5deg); }
  50% { transform: translateY(-5px) rotate(5deg); }
}

.line {
  width: 40px;
  height: 1px;
  background: linear-gradient(90deg, transparent, #ff6b9d, transparent);
}

.heart-container {
  animation: pulse 1.5s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}

.love-message {
  font-family: 'Satisfy', cursive;
  font-size: 1.1rem;
  color: #ff6b9d;
  margin-bottom: 1rem;
  min-height: 2.5em;
  animation: fadeInOut 5s ease-in-out infinite;
  padding: 0 1rem;
}

@keyframes fadeInOut {
  0%, 100% { opacity: 0.7; }
  50% { opacity: 1; }
}

.fun-stats {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  margin-bottom: 1rem;
  flex-wrap: wrap;
}

.stat {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.stat-number {
  font-family: 'Cinzel Decorative', serif;
  font-size: 1.2rem;
  color: #ba68c8;
  font-weight: 700;
}

.stat-label {
  font-size: 0.65rem;
  color: rgba(255, 255, 255, 0.5);
  text-transform: uppercase;
  letter-spacing: 1px;
}

.stat-divider {
  color: rgba(255, 255, 255, 0.3);
}

.credits {
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.6);
  margin-bottom: 0.5rem;
}

.beating-heart {
  display: inline-block;
  animation: heartBeat 1s ease-in-out infinite;
}

@keyframes heartBeat {
  0%, 100% { transform: scale(1); }
  14% { transform: scale(1.3); }
  28% { transform: scale(1); }
  42% { transform: scale(1.3); }
  70% { transform: scale(1); }
}

.twilight-disclaimer {
  font-size: 0.7rem;
  color: rgba(255, 255, 255, 0.4);
  margin-bottom: 0.5rem;
  font-style: italic;
}

.copyright {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.4);
}

.sparkle-footer {
  display: flex;
  justify-content: center;
  gap: 0.75rem;
  margin-top: 1rem;
  opacity: 0.5;
}

.footer-sparkle {
  animation: twinkle 2s ease-in-out infinite;
  font-size: 1rem;
}

.footer-sparkle:nth-child(2) { animation-delay: 0.3s; }
.footer-sparkle:nth-child(3) { animation-delay: 0.6s; }
.footer-sparkle:nth-child(4) { animation-delay: 0.9s; }
.footer-sparkle:nth-child(5) { animation-delay: 1.2s; }
.footer-sparkle:nth-child(6) { animation-delay: 1.5s; }
.footer-sparkle:nth-child(7) { animation-delay: 1.8s; }

@keyframes twinkle {
  0%, 100% { opacity: 0.2; transform: scale(0.8); }
  50% { opacity: 1; transform: scale(1.2); }
}

.easter-egg {
  font-size: 0.65rem;
  color: rgba(255, 255, 255, 0);
  margin-top: 1rem;
  transition: color 0.5s ease;
  cursor: default;
}

.footer:hover .easter-egg {
  color: rgba(255, 255, 255, 0.4);
}
</style>
