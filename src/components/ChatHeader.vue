<script setup>
defineEmits(['close'])
</script>

<template>
  <div class="header">
    <div>
      <div class="logo-section">
        <img src="logo.svg" alt="logo">
        <span>Nitra AI</span>
        <q-icon name="fa-solid fa-wand-magic-sparkles"/>
      </div>
      <p>Hi there, How can we help?</p>
    </div>
    <q-icon name="fa-solid fa-xmark" size="16px" class="close-btn" @click="$emit('close')"/>
  </div>
</template>

<style scoped lang="scss">
.header {
  padding: 18px 20px;
  background-color: $primary;
  display: flex;
  justify-content: space-between;
  color: white;
}

.logo-section {
  font-size: 30px;
  display: flex;
  gap: 16px;
  align-items: center;
}

.close-btn {
  cursor: pointer;
  padding: 10px;
  border-radius: 50%;
  transition: background-color 0.15s ease, transform 0.1s ease;

  &:hover {
    background-color: rgba(255, 255, 255, 0.2);
  }

  &:active {
    background-color: rgba(255, 255, 255, 0.35);
    transform: scale(0.9);
  }
}
</style>
