<script setup>
import { ref } from 'vue'
import { VueSpinner } from 'vue3-spinners'

const props = defineProps({
  isReplying: Boolean
})

const emit = defineEmits(['submit'])

const inputContent = ref('')
const inputRef = ref(null)

const focusInput = () => inputRef.value?.focus()

const onSubmit = () => {
  if (!inputContent.value.trim() || props.isReplying) return
  emit('submit', inputContent.value)
  inputContent.value = ''
}
</script>

<template>
  <div>
    <div class="divider"/>
    <div class="footer" @click="focusInput">
      <label class="input-area">
        <input
          ref="inputRef"
          type="text"
          class="text-input"
          v-model="inputContent"
          placeholder="Say something......"
          :disabled="isReplying"
          @keyup.enter="onSubmit"
        >
      </label>
      <q-icon name="fa-solid fa-paperclip" size="18px" class="clip-icon"/>
      <button class="submit-btn" :disabled="isReplying || !inputContent.trim()" @click="onSubmit">
        <VueSpinner v-if="isReplying" size="18" color="#ffffff"/>
        <q-icon v-else name="fa-solid fa-chevron-right" size="18px"/>
      </button>
    </div>
  </div>
</template>

<style scoped lang="scss">
.divider {
  height: 1px;
  background: $gray-100;
}

.footer {
  display: flex;
  align-items: center;
  padding: 22px;
  gap: 8px;
  background: white;
  cursor: text;
}

.input-area {
  flex: 1;
  align-self: stretch;
  display: flex;
  align-items: center;
  cursor: text;
}

.text-input {
  outline: none;
  width: 100%;
  border: none;
}

.text-input:disabled {
  background-color: white;
  cursor: not-allowed;
}

.clip-icon {
  margin-right: 12px;
  color: $gray-600;
  opacity: 50%;
}

.submit-btn {
  flex-shrink: 0;
  width: 36px;
  height: 36px;
  border: none;
  border-radius: 100px;
  background-color: $primary;
  color: #fff;
  cursor: pointer;
  white-space: nowrap;

  &:disabled {
    background-color: $gray-300;
    cursor: not-allowed;
  }
}
</style>
