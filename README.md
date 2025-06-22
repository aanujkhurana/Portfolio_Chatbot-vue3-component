# 🧠 Vue Portfolio Chatbot

A friendly, smart chatbot you can plug into your personal Vue 3 portfolio. Powered by OpenAI.

## 🚀 Features

- Plug & play Vue 3 chatbot component
- Contextual answers from your own experience, projects & skills
- Powered by OpenAI GPT-3.5 / GPT-4 (via API key)
- Uses Composition API + `<script setup>`
- Fully customizable UI

## 📦 Installation

```bash
npm install
npm run dev
```

## 🔧 Usage

In your Vue app:

```vue
<script setup>
import ChatAssistant from './components/ChatAssistant.vue';
</script>

<template>
  <ChatAssistant :openaiApiKey="'your-api-key'" model="gpt-3.5-turbo" />
</template>
```

Customize `src/data/contextData.ts` to match your profile.

## 🧑‍💻 Author

Made by [Anuj Khurana](https://github.com/anujkhurana)

MIT License
