<script setup lang="ts">
import { ref } from 'vue'
import { useRoute } from 'vue-router'

interface Comment {
  id: number
  content: string
  author: string
  avatar: string
  createTime: string
}

const route = useRoute()
const noteId = route.params.id

const note = ref({
  id: noteId,
  title: '记录一下学习Docker的心得',
  updateTime: '2025-05-15 16:20:00',
  content: `# Docker学习心得

最近开始系统学习Docker，这里记录一下使用心得和注意事项。

## 基本概念

1. 镜像（Image）：Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。

2. 容器（Container）：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。

## 常用命令

### 镜像操作
- docker pull：拉取镜像
- docker images：查看本地镜像
- docker rmi：删除镜像

### 容器操作
- docker run：创建并运行容器
- docker ps：查看容器状态
- docker stop：停止容器
- docker rm：删除容器

## 使用技巧

1. 合理使用缓存层
2. 编写优质的Dockerfile
3. 注意安全性配置

持续学习中...
`,
  likes: 5,
  isLiked: false
})

const comments = ref<Comment[]>([
  {
    id: 1,
    content: '讲解得很清晰，对Docker的概念理解更深入了！',
    author: 'Docker爱好者',
    avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=John',
    createTime: '2025-05-16 09:15:00'
  }
])

const newComment = ref({
  content: ''
})

const addComment = () => {
  if (!newComment.value.content.trim()) return

  comments.value.push({
    id: comments.value.length + 1,
    content: newComment.value.content,
    author: '访客',
    avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${Math.random()}`,
    createTime: new Date().toLocaleString()
  })

  newComment.value.content = ''
}

const toggleLike = () => {
  if (note.value.isLiked) {
    note.value.likes--
  } else {
    note.value.likes++
  }
  note.value.isLiked = !note.value.isLiked
}
</script>

<template>
  <div class="note-detail">
    <h1 class="note-title">{{ note.title }}</h1>
    <div class="note-meta">
      <span class="update-time">{{ note.updateTime }}</span>
    </div>
    
    <div class="note-content markdown-body" v-html="note.content"></div>
    
    <div class="note-actions">
      <button class="like-button" :class="{ 'liked': note.isLiked }" @click="toggleLike">
        <i class="fas fa-heart"></i>
        <span>{{ note.likes }}</span>
      </button>
    </div>

    <div class="comments-section">
      <h2>评论</h2>
      
      <div class="comment-form">
        <textarea
          v-model="newComment.content"
          placeholder="写下你的评论..."
          rows="3"
        ></textarea>
        <button @click="addComment">发表评论</button>
      </div>

      <div class="comments-list">
        <div v-for="comment in comments" :key="comment.id" class="comment-item">
          <img :src="comment.avatar" :alt="comment.author" class="avatar">
          <div class="comment-content">
            <div class="comment-header">
              <span class="author">{{ comment.author }}</span>
              <span class="time">{{ comment.createTime }}</span>
            </div>
            <p class="text">{{ comment.content }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.note-detail {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.note-title {
  font-size: 28px;
  color: #2c3e50;
  margin-bottom: 16px;
}

.note-meta {
  margin-bottom: 24px;
  color: #666;
}

.note-content {
  line-height: 1.8;
  color: #2c3e50;
  margin-bottom: 40px;
}

.note-actions {
  margin-bottom: 40px;
  display: flex;
  justify-content: center;
}

.like-button {
  background: none;
  border: 1px solid #ff4757;
  color: #ff4757;
  padding: 8px 16px;
  border-radius: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: all 0.3s ease;
}

.like-button:hover {
  background: #ff4757;
  color: white;
}

.like-button.liked {
  background: #ff4757;
  color: white;
}

.comments-section {
  margin-top: 40px;
}

.comment-form {
  margin-bottom: 24px;
}

textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  resize: vertical;
  margin-bottom: 12px;
  font-family: inherit;
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

.comments-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.comment-item {
  display: flex;
  gap: 16px;
  padding: 16px;
  background: #f8f9fa;
  border-radius: 8px;
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
}

.comment-content {
  flex: 1;
}

.comment-header {
  margin-bottom: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.author {
  font-weight: 500;
  color: #2c3e50;
}

.time {
  font-size: 12px;
  color: #666;
}

.text {
  margin: 0;
  color: #2c3e50;
  line-height: 1.5;
}
</style>