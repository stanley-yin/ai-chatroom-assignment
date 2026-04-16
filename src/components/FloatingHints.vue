<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const PROMPTS = [
  { text: 'Upload your supplier list',              icon: 'fa-solid fa-list' },
  { text: 'Check if Avastin is in stock',           icon: 'fa-solid fa-cart-shopping' },
  { text: 'Check if there\'s a better price for Xeomin', icon: 'fa-solid fa-hand-holding-dollar' },
  { text: 'What are some generic options for Restylane', icon: 'fa-solid fa-magnifying-glass' },
  { text: 'What\'s the best product for Xeomin',   icon: 'fa-solid fa-thumbs-up' },
]

const index = ref(0)
let timer = null

onMounted(() => {
  timer = setInterval(() => {
    index.value = (index.value + 1) % PROMPTS.length
  }, 2500)
})

onUnmounted(() => clearInterval(timer))
</script>

<template>
  <div class="hints-wrapper">
    <div
      :key="index"
      class="slide-up hints-item"
    >
      <q-icon :name="PROMPTS[index].icon" size="14px" class="hint-icon"/>
      {{ PROMPTS[index].text }}
    </div>
  </div>
</template>

<style scoped lang="scss">
.hints-wrapper {
  position: relative;
  width: 100%;
  overflow: hidden;
}

.hints-item {
  display: flex;
  align-items: center;
  gap: 8px;
  color: $gray-700;
}

.hint-icon {
  color: $teal-300;
}

@keyframes slide-up {
  0%   { transform: translateY(100%); opacity: 0; }
  15%  { transform: translateY(0);    opacity: 1; }
  85%  { transform: translateY(0);    opacity: 1; }
  100% { transform: translateY(-100%); opacity: 0; }
}

.slide-up {
  animation: slide-up 2.5s ease-in-out;
}
</style>
