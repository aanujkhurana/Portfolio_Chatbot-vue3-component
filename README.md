# Vue Portfolio Chatbot
![npm](https://img.shields.io/npm/v/vue-portfolio-chatbot) ![license](https://img.shields.io/npm/l/vue-portfolio-chatbot)


A reusable, friendly Vue 3 component powered by OpenAI that adds a smart portfolio assistant to your personal website.  
Ask about your skills, projects, experience, or anything you define — it responds with context from your own data.

<img src="https://github.com/aanujkhurana/ai-chatbox-vue-component/blob/8edc0e279fc9fde60546bd7eedce42aa2dea0dcc/public/image.png?raw=true" width="300" alt="chatbot demo" />

---

## ✨ Features

- 🔧 Built with Vue 3 + TypeScript
- 🧠 Powered by OpenAI GPT (3.5/4)
- 💬 Realtime chat with typing indicator
- 🪄 Dynamic system prompt based on your skills & projects
- 🔌 Plug-and-play: Drop into any Vue app
- 🧩 Fully customizable UI and data
- 🌐 Can run in "demo mode" without OpenAI key

---

## 🚀 Quick Start

### 1. Install

```bash
npm install vue-portfolio-chatbot
````

### 2. Use in your Vue app

```ts
<script setup lang="ts">
import ChatAssistant from 'vue-portfolio-chatbot';
</script>

<template>
  <ChatAssistant openaiApiKey="your-openai-api-key" />
</template>
```

> 💡 Leave `openaiApiKey` blank for demo mode (no real API calls).

---

## 📁 Folder Structure

```
src/
├── components/
│   └── ChatAssistant.vue  # Main chatbot component
├── data/
│   ├── contextData.ts     # Your custom profile, skills, and projects
│   └── systemPrompt.ts    # System prompt builder from contextData
└── index.ts               # Component export for npm
```

---

## 🔐 OpenAI API Key (Optional)

To get actual AI responses:

1. Sign up at [platform.openai.com](https://platform.openai.com)
2. Create an API key
3. Pass it like:

```html
<ChatAssistant openaiApiKey="sk-..." model="gpt-3.5-turbo" />
```

---

## 🧠 Customization

You can fully customize:

* Your personal `contextData.ts`
* Prompt logic in `systemPrompt.ts`
* Styles (comes with scoped CSS)
* UI structure in `ChatAssistant.vue`

Want to use it in a **React, Nuxt, or Svelte** project? DM me — a universal wrapper is coming!

---

## 📄 License

MIT — Feel free to use, fork, or improve!

---

## ⭐ Like this?

Give it a star ⭐ and share your portfolio using it! Contributions & feedback welcome.
