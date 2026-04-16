<script setup>
import { computed, reactive, ref } from 'vue'
import { MESSAGE_MOCK_MAP } from '../mock/messages.js'
import MessageItem from 'components/MessageItem.vue'
import FloatingHints from 'components/FloatingHints.vue'
import ChatHeader from 'components/ChatHeader.vue'
import ChatFooter from 'components/ChatFooter.vue'

const emit = defineEmits(['close'])

let msgIdCounter = 0

const welcomeMessage = {
  id: msgIdCounter++,
  role: 'assistant',
  content: 'Welcome to Nitra AI!',
  skipThinking: true
}

const chatList = reactive([welcomeMessage])
const messageQueue = reactive([])
const scrollTarget = ref(null)
const pendingResolvers = {}
const isReplying = ref(false)
const hasUserMessage = computed(() => chatList.some(m => m.role === 'user'))

const waitForDone = (id) => new Promise(resolve => {
  pendingResolvers[id] = resolve
})

const onMessageDone = (id) => {
  const resolve = pendingResolvers[id]
  resolve && resolve()
  delete pendingResolvers[id]
}

const processQueue = async () => {
  isReplying.value = true
  while (messageQueue.length > 0) {
    const next = messageQueue.shift()
    chatList.push(next)
    await waitForDone(next.id)
  }
  isReplying.value = false
}

const parseReply = (content) => {
  return content.split(/Suggested?\s+Question[：:]\s*/i).map(s => s.trim()).filter(Boolean)
}

const onSubmit = (text) => {
  chatList.push({
    id: msgIdCounter++,
    role: 'user',
    content: text,
    timestamp: Date.now()
  })

  const reply = MESSAGE_MOCK_MAP[text]
  const parts = reply
    ? parseReply(reply.message.content)
    : ["Sorry, I don't understand that question."]

  parts.forEach((part, index) => {
    messageQueue.push({
      id: msgIdCounter++,
      role: 'assistant',
      content: part,
      skipThinking: index > 0,
      timestamp: Date.now()
    })
  })

  processQueue()
}
</script>

<template>
  <div class="chatbox">
    <ChatHeader @close="emit('close')"/>
    <div class="content">
      <div class="messages" ref="scrollTarget">
        <MessageItem
          v-for="item in chatList"
          :key="item.id"
          :msg-id="item.id"
          :text="item.content"
          :is-me="item.role === 'user'"
          :skip-thinking="item.skipThinking"
          :scroll-target="scrollTarget"
          @done="onMessageDone"
        />
      </div>
      <FloatingHints v-if="!hasUserMessage" class="floating-hints"/>
    </div>
    <ChatFooter :is-replying="isReplying" @submit="onSubmit"/>
  </div>
</template>

<style scoped lang="scss">
.chatbox {
  width: 780px;
  display: flex;
  flex-direction: column;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 30px 60px 0 rgba(38, 77, 79, 0.25);
}

.content {
  height: 420px;
  position: relative;
  flex-grow: 1;
  background: #ffffff;
  overflow: hidden;
}

.messages {
  height: 100%;
  padding: 20px;
  overflow-y: auto;
  box-sizing: border-box;
}

.floating-hints {
  position: absolute;
  bottom: 12px;
  left: 20px;
  right: 20px;
  pointer-events: none;
}
</style>
