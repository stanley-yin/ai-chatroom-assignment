<script setup>
defineProps({
  text: String
})
</script>

<template>
  <button class="help-button">
    <q-icon name="fa-solid fa-wand-magic-sparkles" />
    {{ text }}
  </button>
</template>

<style scoped lang="scss">
.help-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  border: none;
  border-radius: 999px;
  background-color: $orange-400;
  box-shadow: 0px 4px 6px -4px #0000001A;
  color: white;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.15s ease, box-shadow 0.15s ease, transform 0.1s ease;

  &:hover {
    background-color: $orange-500;
    box-shadow: 0px 10px 15px -3px #0000001A;
  }

  &:active {
    background-color: $orange-600;
    box-shadow: none;
    transform: scale(0.97);
  }
}
</style>
