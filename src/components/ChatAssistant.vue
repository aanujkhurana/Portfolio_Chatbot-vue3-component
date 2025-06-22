// Placeholder: Add your ChatAssistant.vue code here
<!-- src/components/ChatAssistant.vue -->
<template>
  <div class="chatbox" :class="{ open: isOpen }">
    <button class="toggle-btn" @click="toggleChat">
      💬
    </button>

    <div class="chat-window" v-if="isOpen">
      <div class="header">Ask Me Anything</div>

      <div class="messages" ref="messagesContainer">
        <div v-for="msg in messages" :key="msg.id" :class="['msg', msg.type]">
          <span v-html="formatMessage(msg.text)" />
        </div>
        <div v-if="isTyping" class="typing">Assistant is typing...</div>
      </div>

      <form @submit.prevent="handleSendMessage">
        <input
          ref="messageInput"
          v-model="currentMessage"
          type="text"
          placeholder="Ask about my projects, skills..."
        />
        <button type="submit">➤</button>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, nextTick } from 'vue';
import { buildSystemPrompt } from '../data/systemPrompt';
import { contextData } from '../data/contextData';

const props = defineProps<{
  openaiApiKey?: string;
  model?: string;
}>();

const isOpen = ref(false);
const messages = ref<any[]>([]);
const currentMessage = ref('');
const isTyping = ref(false);
const messagesContainer = ref<HTMLElement | null>(null);
const messageInput = ref<HTMLElement | null>(null);
const conversationHistory = ref<any[]>([]);

// Welcome
onMounted(() => {
  messages.value.push({
    id: 'welcome',
    type: 'bot',
    text: "Hi, I'm your portfolio assistant! Ask me about Anuj’s skills, projects, or experience.",
    timestamp: new Date(),
  });
});

watch(
  () => messages.value.length,
  async () => {
    await nextTick();
    messagesContainer.value?.scrollTo({
      top: messagesContainer.value.scrollHeight,
      behavior: 'smooth',
    });
  }
);

const toggleChat = () => {
  isOpen.value = !isOpen.value;
  if (isOpen.value) {
    nextTick(() => messageInput.value?.focus());
  }
};

const formatMessage = (text: string) =>
  text
    .replace(/\*\*([^*]+)\*\*/g, '<strong>$1</strong>')
    .replace(/\n/g, '<br>');

// API Call with Retry
const callOpenAI = async (userMessage: string, retry = 0): Promise<string> => {
  const payload = {
    model: props.model || 'gpt-3.5-turbo',
    messages: [
      { role: 'system', content: buildSystemPrompt() },
      ...conversationHistory.value,
      { role: 'user', content: userMessage },
    ],
    temperature: 0.7,
    max_tokens: 500,
  };

  const res = await fetch('https://api.openai.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${props.openaiApiKey}`,
    },
    body: JSON.stringify(payload),
  });

  if (res.status === 429 && retry < 3) {
    await new Promise((res) => setTimeout(res, 1000 * (retry + 1)));
    return callOpenAI(userMessage, retry + 1);
  }

  if (!res.ok) {
    throw new Error(`OpenAI API error: ${res.status}`);
  }

  const data = await res.json();
  return data.choices[0].message.content;
};

const handleSendMessage = async () => {
  if (!currentMessage.value.trim() || isTyping.value) return;

  const userText = currentMessage.value.trim();
  messages.value.push({
    id: `user_${Date.now()}`,
    type: 'user',
    text: userText,
    timestamp: new Date(),
  });
  currentMessage.value = '';
  isTyping.value = true;

  try {
    const response = props.openaiApiKey
      ? await callOpenAI(userText)
      : "🤖 (Demo mode) I can't answer questions without an API key.";

    messages.value.push({
      id: `bot_${Date.now()}`,
      type: 'bot',
      text: response,
      timestamp: new Date(),
    });

    conversationHistory.value.push(
      { role: 'user', content: userText },
      { role: 'assistant', content: response }
    );
    conversationHistory.value = conversationHistory.value.slice(-20);
  } catch (err: any) {
    messages.value.push({
      id: `err_${Date.now()}`,
      type: 'bot',
      text: err.message,
      timestamp: new Date(),
    });
  } finally {
    isTyping.value = false;
  }
};
</script>

<style scoped>
.chatbox {
  position: fixed;
  bottom: 20px;
  right: 20px;
  font-family: sans-serif;
}
.toggle-btn {
  background: #0d1117;
  color: white;
  padding: 0.5em 1em;
  border-radius: 50px;
  border: none;
  cursor: pointer;
}
.chat-window {
  width: 300px;
  height: 400px;
  background: white;
  border-radius: 10px;
  border: 1px solid #ccc;
  margin-top: 10px;
  display: flex;
  flex-direction: column;
}
.header {
  background: #f3f4f6;
  padding: 0.7em;
  font-weight: bold;
  border-bottom: 1px solid #ccc;
}
.messages {
  flex: 1;
  padding: 1em;
  overflow-y: auto;
}
.msg {
  margin-bottom: 1em;
}
.msg.user {
  text-align: right;
}
.msg.bot {
  text-align: left;
}
.typing {
  font-style: italic;
  color: gray;
}
form {
  display: flex;
  border-top: 1px solid #ccc;
}
input {
  flex: 1;
  padding: 0.5em;
  border: none;
}
button[type='submit'] {
  background: #0d1117;
  color: white;
  border: none;
  padding: 0.5em 1em;
}
</style>
