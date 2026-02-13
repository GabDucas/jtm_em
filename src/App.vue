<script setup>
import { ref, onMounted } from 'vue'
import Header from './components/Header.vue'
import QuoteBanner from './components/QuoteBanner.vue'
import CategoryGrid from './components/CategoryGrid.vue'
import SurpriseButton from './components/SurpriseButton.vue'
import IdeaModal from './components/IdeaModal.vue'
import FloatingElements from './components/FloatingElements.vue'
import TwilightCorner from './components/TwilightCorner.vue'
import Footer from './components/Footer.vue'
import { getRandomIdea, getRandomIdeaFromAll } from './data/dates.js'

const showModal = ref(false)
const currentIdea = ref(null)
const currentCategoryId = ref(null)

function handleCategoryClick(categoryId) {
  currentCategoryId.value = categoryId
  currentIdea.value = getRandomIdea(categoryId)
  showModal.value = true
}

function handleSurprise() {
  currentCategoryId.value = null
  currentIdea.value = getRandomIdeaFromAll()
  showModal.value = true
}

function handleRegenerate() {
  if (currentCategoryId.value) {
    currentIdea.value = getRandomIdea(currentCategoryId.value)
  } else {
    currentIdea.value = getRandomIdeaFromAll()
  }
}

function closeModal() {
  showModal.value = false
}
</script>

<template>
  <div class="app-container">
    <FloatingElements />

    <Header />

    <QuoteBanner />

    <TwilightCorner />

    <CategoryGrid @category-click="handleCategoryClick" />

    <SurpriseButton @click="handleSurprise" />

    <Footer />

    <IdeaModal
      :show="showModal"
      :idea="currentIdea"
      @close="closeModal"
      @regenerate="handleRegenerate"
    />
  </div>
</template>

<style scoped>
.app-container {
  max-width: 480px;
  margin: 0 auto;
  padding: 1rem;
  min-height: 100vh;
  position: relative;
}
</style>
