<script setup>
import { dateCategories } from '../data/dates.js'

const emit = defineEmits(['category-click'])

function handleClick(categoryId) {
  emit('category-click', categoryId)
}
</script>

<template>
  <section class="categories-section">
    <h2 class="section-title">
      <span>🦇</span>
      Choisis ton aventure, mon amour éternel
      <span>🦇</span>
    </h2>

    <div class="categories-grid">
      <button
        v-for="category in dateCategories"
        :key="category.id"
        class="category-card"
        :style="{
          '--card-color': category.color,
          '--card-gradient': category.gradient
        }"
        @click="handleClick(category.id)"
      >
        <div class="card-glow"></div>
        <span class="category-icon">{{ category.icon }}</span>
        <span class="category-icon-2">{{ category.emoji2 }}</span>
        <h3 class="category-name">{{ category.name }}</h3>
        <p class="category-count">{{ category.ideas.length }} idées magiques ✨</p>
        <div class="card-sparkles">
          <span v-for="n in 3" :key="n" class="sparkle-dot">•</span>
        </div>
      </button>
    </div>
  </section>
</template>

<style scoped>
.categories-section {
  position: relative;
  z-index: 1;
  margin-top: 1rem;
}

.section-title {
  font-family: 'Satisfy', cursive;
  font-size: 1.2rem;
  text-align: center;
  margin-bottom: 1.5rem;
  color: rgba(255, 255, 255, 0.8);
  font-weight: 400;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.section-title span {
  animation: batFly 2s ease-in-out infinite;
}

@keyframes batFly {
  0%, 100% { transform: translateY(0) rotate(-5deg); }
  50% { transform: translateY(-5px) rotate(5deg); }
}

.categories-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
}

.category-card {
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  padding: 1.5rem 1rem;
  text-align: center;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  position: relative;
  overflow: hidden;
  backdrop-filter: blur(10px);
}

.card-glow {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: var(--card-gradient);
  opacity: 0;
  transition: opacity 0.4s ease;
  z-index: 0;
}

.category-card:hover .card-glow,
.category-card:active .card-glow {
  opacity: 0.2;
}

.category-card:hover {
  transform: translateY(-8px) scale(1.03);
  border-color: var(--card-color);
  box-shadow:
    0 20px 40px rgba(0, 0, 0, 0.3),
    0 0 30px color-mix(in srgb, var(--card-color) 30%, transparent);
}

.category-card:active {
  transform: translateY(-2px) scale(0.98);
}

.category-icon {
  font-size: 3rem;
  display: block;
  position: relative;
  z-index: 1;
  transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.category-icon-2 {
  font-size: 1.5rem;
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  opacity: 0.6;
  transition: all 0.3s ease;
}

.category-card:hover .category-icon {
  transform: scale(1.2) rotate(10deg);
  animation: wiggle 0.5s ease-in-out;
}

.category-card:hover .category-icon-2 {
  opacity: 1;
  transform: scale(1.2);
}

@keyframes wiggle {
  0%, 100% { transform: scale(1.2) rotate(10deg); }
  25% { transform: scale(1.2) rotate(-5deg); }
  75% { transform: scale(1.2) rotate(15deg); }
}

.category-name {
  font-family: 'Cinzel Decorative', serif;
  font-size: 0.95rem;
  font-weight: 600;
  position: relative;
  z-index: 1;
  color: white;
  margin-top: 0.5rem;
}

.category-count {
  font-size: 0.7rem;
  color: rgba(255, 255, 255, 0.6);
  margin-top: 0.25rem;
  position: relative;
  z-index: 1;
}

.card-sparkles {
  position: absolute;
  bottom: 0.5rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 0.25rem;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.category-card:hover .card-sparkles {
  opacity: 1;
}

.sparkle-dot {
  color: var(--card-color);
  animation: sparkleFloat 1s ease-in-out infinite;
  font-size: 1.5rem;
}

.sparkle-dot:nth-child(2) {
  animation-delay: 0.2s;
}

.sparkle-dot:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes sparkleFloat {
  0%, 100% { transform: translateY(0); opacity: 0.5; }
  50% { transform: translateY(-5px); opacity: 1; }
}
</style>
