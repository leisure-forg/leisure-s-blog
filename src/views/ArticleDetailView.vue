<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'

interface Comment {
  id: number
  content: string
  author: string
  avatar: string
  createTime: string
}

// 定义Article接口
interface Article {
  id: number
  title: string
  updateTime: string
  content: string
  tags: string[]
  likes: number
  isLiked: boolean
}

const route = useRoute()
const articleId = route.params.id as string | undefined

// 模拟文章数据，明确类型
const articlesData: Record<string, Article> = {
  '1': {
    id: 1,
    title: 'Redis解决缓存击穿、缓存穿透方法',
    updateTime: '2025-05-09 15:53:59',
    content: `# Redis缓存问题解决方案

## 什么是缓存击穿？

缓存击穿是指某个热点key在过期的一瞬间，持续的大并发请求就穿透到数据库，导致数据库瞬间压力过大。

## 解决方案

1. 互斥锁
2. 热点数据永不过期
3. 资源保护

## 具体实现

使用Redis的SETNX命令实现互斥锁：

\`\`\`java
if (redis.setnx(key_mutex, 1, timeout) == 1) {
    try {
        // 从数据库中获取数据
        value = db.get(key);
        // 将数据设置到缓存
        redis.set(key, value, timeout);
        return value;
    } finally {
        // 释放锁
        redis.del(key_mutex);
    }
} else {
    // 其他线程等待
    Thread.sleep(50);
    return get(key);
}
\`\`\`
`,
    tags: ['SpringBoot', 'Redis'],
    likes: 2,
    isLiked: false,
  },
  '2': {
    id: 2,
    title: 'Spring Security整合JWT实现认证授权',
    updateTime: '2025-05-08 14:30:00',
    content: `Spring Security与JWT整合指南

## 项目配置

首先需要添加相关依赖,配置Security和JWT的基础设施...

## 实现步骤

1. 配置Security
2. 实现JWT工具类
3. 自定义认证过滤器
4. 配置接口权限

## 测试验证

使用Postman进行接口测试...
`,
    tags: ['Spring Security', 'JWT'],
    likes: 1,
    isLiked: false,
  },
}

// 明确ref的类型
const article = ref<Article | null>(null)

onMounted(() => {
  // 处理articleId可能为undefined的情况
  if (!articleId) {
    console.error('文章ID不存在')
    return
  }

  const articleData = articlesData[articleId]
  if (articleData) {
    article.value = articleData
  } else {
    console.error('文章不存在')
  }
})

const comments = ref<Comment[]>([
  {
    id: 1,
    content: '文章写得很详细，对我帮助很大！',
    author: '技术爱好者',
    avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Felix',
    createTime: '2025-05-10 10:30:00',
  },
])

const newComment = ref({
  content: '',
})

const formatDate = (date: Date): string => {
  const pad = (n: number) => String(n).padStart(2, '0')
  return `${date.getFullYear()}-${pad(date.getMonth() + 1)}-${pad(date.getDate())} ${pad(date.getHours())}:${pad(date.getMinutes())}:${pad(date.getSeconds())}`
}

const addComment = () => {
  const content = newComment.value.content.trim()

  // 内容验证
  if (!content) return
  if (content.length > 500) {
    alert('评论内容不能超过500字')
    return
  }

  // 生成更可靠的唯一ID
  const newId = comments.value.length > 0 ? Math.max(...comments.value.map((c) => c.id)) + 1 : 1

  comments.value.push({
    id: newId,
    content: content,
    author: '访客',
    avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${Date.now()}`,
    createTime: formatDate(new Date()),
  })

  newComment.value.content = ''
}

const toggleLike = () => {
  if (article.value == null) return

  // 确保likes不会变成负数
  if (article.value.isLiked) {
    article.value.likes = Math.max(0, article.value.likes - 1)
  } else {
    article.value.likes++
  }
  article.value.isLiked = !article.value.isLiked
}
</script>

<template>
  <div v-if="article" class="article-detail">
    <h1 class="article-title">{{ article.title }}</h1>
    <div class="article-meta">
      <span class="update-time">{{ article.updateTime }}</span>
      <span v-for="tag in article.tags" :key="tag" class="tag">{{ tag }}</span>
    </div>

    <div class="article-content markdown-body" v-html="article.content"></div>

    <div class="article-actions">
      <button class="like-button" :class="{ liked: article.isLiked }" @click="toggleLike">
        <i class="fas fa-heart"></i>
        <span>{{ article.likes }}</span>
      </button>
    </div>

    <div class="comments-section">
      <h2>评论</h2>

      <div class="comment-form">
        <textarea v-model="newComment.content" placeholder="写下你的评论..." rows="3"></textarea>
        <button @click="addComment">发表评论</button>
      </div>

      <div class="comments-list">
        <div v-for="comment in comments" :key="comment.id" class="comment-item">
          <img :src="comment.avatar" :alt="comment.author" class="avatar" />
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
.article-detail {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.error-message {
  text-align: center;
  padding: 40px;
  color: #ff4757;
  font-size: 18px;
}

.error-message i {
  font-size: 48px;
  margin-bottom: 16px;
}

.error-message p {
  margin: 16px 0;
}

.back-button {
  background: #ff4757;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.back-button:hover {
  background: #ff6b81;
}

.article-title {
  font-size: 28px;
  color: #2c3e50;
  margin-bottom: 16px;
}

.article-meta {
  margin-bottom: 24px;
}

.update-time {
  color: #666;
  margin-right: 16px;
}

.tag {
  background: #f0f2f5;
  padding: 4px 8px;
  border-radius: 4px;
  margin-right: 8px;
  font-size: 12px;
  color: #666;
}

.article-content {
  line-height: 1.8;
  color: #2c3e50;
  margin-bottom: 40px;
}

.article-actions {
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
