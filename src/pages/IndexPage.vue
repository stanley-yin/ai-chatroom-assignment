<script setup>
import HelpButton from "components/HelpButton.vue";
import Chatbox from "components/Chatbox.vue";
import { ref } from "vue";
import { MESSAGE_MOCK_MAP } from "src/mock/messages.js";

const isOpen = ref(false)
const questions = Object.keys(MESSAGE_MOCK_MAP)
</script>

<template>
  <q-page class="flex flex-center">
    <div class="questions-list">
      <p class="questions-title">Sample Questions</p>
      <ul>
        <li v-for="q in questions" :key="q">{{ q }}</li>
      </ul>
    </div>
    <HelpButton class="help-button-fixed" text="Ask Nitra AI" @click="isOpen = !isOpen"/>
    <Transition name="chatbox">
      <Chatbox v-show="isOpen" class="chatbox-fixed" @close="isOpen = false"/>
    </Transition>
  </q-page>
</template>

<style scoped lang="scss">
.questions-list {
  max-width: 480px;
  width: 100%;
}

.questions-title {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 12px;
  color: $gray-800;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;

  li {
    padding: 12px 16px;
    border-radius: 8px;
    background-color: $gray-0;
    border: 1px solid $gray-100;
    color: $gray-700;
    font-size: 14px;
    line-height: 1.5;
  }
}

.help-button-fixed {
  position: fixed;
  bottom: 32px;
  right: 32px;
}

.chatbox-fixed {
  position: fixed;
  bottom: 90px;
  right: 32px;
}

.chatbox-enter-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}

.chatbox-leave-active {
  transition: opacity 0.2s ease, transform 0.2s ease;
}

.chatbox-enter-from {
  opacity: 0;
  transform: translateY(20px) scale(0.97);
}

.chatbox-leave-to {
  opacity: 0;
  transform: translateY(20px) scale(0.97);
}
</style>
