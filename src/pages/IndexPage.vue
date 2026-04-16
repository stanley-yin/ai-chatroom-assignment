<script setup>
import HelpButton from "components/HelpButton.vue";
import Chatbox from "components/Chatbox.vue";
import {ref} from "vue";

const isOpen = ref(false)
</script>

<template>
  <q-page class="flex flex-center">
    <p>Index Page</p>
    <HelpButton class="help-button-fixed" text="Ask Nitra AI" @click="isOpen = !isOpen"/>
    <Transition name="chatbox">
      <Chatbox v-if="isOpen" class="chatbox-fixed" @close="isOpen = false"/>
    </Transition>
  </q-page>
</template>

<style scoped>
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
