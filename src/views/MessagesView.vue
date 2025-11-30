<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface Message {
  id: number
  content: string
  author: string
  avatar: string
  createTime: string
  color: string
}

const messages = ref<Message[]>([])
const loading = ref(false)
const submitting = ref(false)
const currentUser = ref<{
  id: number
  nickname?: string
  username?: string
  avatar_url?: string
} | null>(null)

const BASE_URL = 'http://139.196.162.210:8080'

const colors = ref([
  '#fff8dc', // 淡黄色
  '#e6f3ff', // 淡蓝色
  '#fff0f5', // 淡粉色
  '#f0fff0', // 淡绿色
  '#f5f5f5', // 淡灰色
  '#ffe4e1', // 淡红色
  '#f0f8ff', // 淡天蓝色
])

const newMessage = ref({
  content: '',
})

const getToken = () => localStorage.getItem('token')

const fetchUserInfo = async () => {
  const token = getToken()
  if (!token) return
  try {
    const response = await fetch(`${BASE_URL}/api/v1/users/me`, {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })
    if (response.ok) {
      currentUser.value = await response.json()
    }
  } catch (error) {
    console.error('Fetch user info error:', error)
  }
}

const fetchMessages = async () => {
  loading.value = true
  try {
    const response = await fetch(`${BASE_URL}/api/v1/guestbook`)
    if (response.ok) {
      const data = await response.json()
      messages.value = data.map((item: any, index: number) => ({
        id: item.id,
        content: item.content,
        author: item.nickname || '访客',
        avatar: item.avatar_url || `https://api.dicebear.com/7.x/avataaars/svg?seed=${item.id}`,
        createTime: new Date(item.created_at).toLocaleString(),
        color: colors.value[index % colors.value.length], // 循环分配颜色
      }))
    }
  } catch (error) {
    console.error('Fetch messages error:', error)
  } finally {
    loading.value = false
  }
}

const addMessage = async () => {
  if (!newMessage.value.content.trim()) return

  submitting.value = true
  try {
    const token = getToken()
    if (!token) {
      alert('请先登录')
      return
    }

    await fetchUserInfo()

    const payload = {
      user_id: currentUser.value?.id || 0,
      content: newMessage.value.content,
      nickname: currentUser.value?.nickname || currentUser.value?.username || '访客',
      avatar_url: currentUser.value?.avatar_url,
    }

    const response = await fetch(`${BASE_URL}/api/v1/guestbook`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`,
      },
      body: JSON.stringify(payload),
    })

    if (response.ok) {
      newMessage.value.content = ''
      await fetchMessages()
    } else {
      alert('留言失败')
    }
  } catch (error) {
    console.error('Add message error:', error)
    alert('留言出错')
  } finally {
    submitting.value = false
  }
}

onMounted(() => {
  fetchUserInfo()
  fetchMessages()
})
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
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 8px;
  object-fit: cover;
}

.author {
  font-weight: 500;
  color: #2bd814;
}

.message-content {
  font-size: 14px;
  line-height: 1.5;
  margin-bottom: 12px;
  color: #22c0e7;
}

.message-time {
  font-size: 12px;
  color: var(--color-background);
  text-align: right;
}
</style>
