<script setup lang="ts">
import { ref } from 'vue'

interface Message {
  id: number
  content: string
  author: string
  avatar: string
  createTime: string
  color: string
}

const messages = ref<Message[]>([
  {
    id: 1,
    content: '博主的文章写得很棒，希望能多分享一些技术文章！',
    author: '技术爱好者',
    avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Felix',
    createTime: '2025-05-20 15:30:00',
    color: '#fff8dc',
  },
  {
    id: 2,
    content: '最近的Docker教程对我帮助很大，感谢分享！',
    author: 'Docker学习者',
    avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=John',
    createTime: '2025-05-19 14:20:00',
    color: '#e6f3ff',
  },
  {
    id: 3,
    content: '期待更多Spring Security相关的内容！',
    author: 'Spring开发者',
    avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Alice',
    createTime: '2025-05-18 10:15:00',
    color: '#fff0f5',
  },
])

const colors = ref([
  'var(--color-background-mute)',
  'var(--color-background-soft)',
  'var(--color-background-mute)',
  'var(--color-background-soft)',
  'var(--color-background-mute)',
])

const newMessage = ref({
  content: '',
  author: '访客',
})

const addMessage = () => {
  if (!newMessage.value.content.trim()) return

  messages.value.unshift({
    id: messages.value.length + 1,
    content: newMessage.value.content,
    author: newMessage.value.author,
    avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${Math.random()}`,
    createTime: new Date().toLocaleString(),
    color: colors[Math.floor(Math.random() * colors.value.length)],
  })

  newMessage.value.content = ''
}
</script>

<template>
  <div class="messages-container">
    <h1 class="page-title">留言板</h1>

    <div class="message-form">
      <textarea v-model="newMessage.content" placeholder="写下你的留言..." rows="3"></textarea>
      <button @click="addMessage">发送留言</button>
    </div>

    <div class="messages-wall">
      <div
        v-for="message in messages"
        :key="message.id"
        class="message-note"
        :style="{ backgroundColor: message.color }"
      >
        <div class="message-header">
          <img :src="message.avatar" :alt="message.author" class="avatar" />
          <span class="author">{{ message.author }}</span>
        </div>
        <div class="message-content">{{ message.content }}</div>
        <div class="message-time">{{ message.createTime }}</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.messages-container {
  padding: 20px;
}

.page-title {
  font-size: 24px;
  margin-bottom: 20px;
  color: var(--color-heading);
}

.message-form {
  margin-bottom: 30px;
  background: var(--color-background-soft);
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
}

textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid var(--color-border);
  border-radius: 8px;
  resize: vertical;
  margin-bottom: 12px;
  font-family: inherit;
  background-color: var(--color-background);
  color: var(--color-text);
}

button {
  background: #4a90e2;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.2s;
}

button:hover {
  background: #357abd;
}

.messages-wall {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  padding: 20px 0;
}

.message-note {
  padding: 16px;
  border-radius: 8px;
  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);
  transform: rotate(-1deg);
  transition: transform 0.2s;
}

.message-note:nth-child(even) {
  transform: rotate(1deg);
}

.message-note:hover {
  transform: rotate(0) scale(1.02);
  z-index: 1;
}

.message-header {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.avatar {
  width: 24px;
  height: 24px;
  border-radius: 50%;
  margin-right: 8px;
}

.author {
  font-weight: 500;
  color: #2f2f46;
}

.message-content {
  font-size: 14px;
  line-height: 1.5;
  margin-bottom: 12px;
  color: #2f2f46;
}

.message-time {
  font-size: 12px;
  color: var(--color-text-light);
  text-align: right;
}
</style>
