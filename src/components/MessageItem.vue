<script setup>
import {computed, nextTick, onMounted, onUnmounted, ref, watch} from "vue";
import MarkdownIt from 'markdown-it'
import hljs from 'highlight.js'

const md = new MarkdownIt({
  html: false,
  linkify: true,
  typographer: true,
  highlight: function (str, lang) {
    if (lang && hljs.getLanguage(lang)) {
      try {
        return hljs.highlight(str, {language: lang}).value;
      } catch (__) {}
    }
    return '';
  }
})

const props = defineProps({
  msgId: Number,
  text: String,
  isMe: Boolean,
  skipThinking: Boolean,
  scrollTarget: Object
})

const emit = defineEmits(['done'])

const displayText = ref('')
const isThinking = ref(false)
let timer = null

const simulateBotResponse = async () => {
  if (!props.skipThinking) {
    isThinking.value = true
    displayText.value = ''
    await new Promise(resolve => setTimeout(resolve, 1500))
    isThinking.value = false
  }

  const text = props.text
  let index = 0

  timer = setInterval(() => {
    if (index < text.length) {
      displayText.value += text.charAt(index)
      index++
    } else {
      clearInterval(timer)
      timer = null
      emit('done', props.msgId)
    }
  }, 5)
}

watch(displayText, () => {
  nextTick(() => {
    const el = props.scrollTarget
    if (!el) return
    const isNearBottom = el.scrollHeight - el.scrollTop - el.clientHeight < 80
    if (isNearBottom) {
      el.scrollTop = el.scrollHeight
    }
  })
})

const renderedMarkdown = computed(() => md.render(displayText.value))

onMounted(() => {
  if (props.isMe) {
    displayText.value = props.text
  } else {
    simulateBotResponse()
  }
})

onUnmounted(() => {
  if (timer) {
    clearInterval(timer)
    timer = null
  }
})
</script>

<template>
  <div class="message-container" :class="{ 'is-me': isMe }">
    <div v-if="isThinking" class="thinking-state">
      Thinking
      <span class="dot-animation">...</span>
    </div>
    <div v-else class="markdown-body bubble" v-html="renderedMarkdown"></div>
  </div>
</template>


<style scoped lang="scss">
.message-container {
  display: flex;
  justify-content: flex-start;
  margin-bottom: 10px;
}

.message-container.is-me {
  justify-content: flex-end;
}

.bubble {
  max-width: 80%;
  padding: 10px;
  border-radius: 0 10px 10px 10px;
  background-color: $gray-100;
}

.is-me .bubble {
  background-color: $blue-100;
  border-radius:  10px 0 10px 10px;
}

.dot-animation {
  display: inline-block;
  font-weight: bold;
  animation: dot-blink 1.4s infinite both;
}

@keyframes dot-blink {
  0% { opacity: .2; }
  20% { opacity: 1; }
  100% { opacity: .2; }
}

.thinking-state {
  background: $gray-100;
  padding: 8px 16px;
  border-radius: 12px;
  color: $gray-600;
  font-style: italic;
  display: inline-flex;
  align-items: center;
}
</style>
