<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'
import { marked } from 'marked'

interface Image {
  id: number
  url: string
  alt: string
  position: 'left' | 'center' | 'right' | 'full'
}

interface Comment {
  id: number
  content: string
  author: string
  avatar: string
  createTime: string
}

const route = useRoute()
const noteId = route.params.id

// 模拟笔记内容，包含图片
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
  // 添加图片数组
  images: [
    {
      id: 1,
      url: 'https://picsum.photos/800/400?random=1',
      alt: 'Docker架构图',
      position: 'full', // 全宽显示
    },
    {
      id: 2,
      url: 'https://picsum.photos/400/300?random=2',
      alt: 'Docker容器示例',
      position: 'right', // 右侧显示
    },
    {
      id: 3,
      url: 'https://picsum.photos/400/300?random=3',
      alt: 'Docker命令示例',
      position: 'left', // 左侧显示
    },
  ] as Image[],
  likes: 5,
  isLiked: false,
})

const comments = ref<Comment[]>([
  {
    id: 1,
    content: '讲解得很清晰，对Docker的概念理解更深入了！',
    author: 'Docker爱好者',
    avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=John',
    createTime: '2025-05-16 09:15:00',
  },
])

const newComment = ref({
  content: '',
})

// 图片插入位置映射
const imagePositions = {
  Docker学习心得: 0, // 在标题后插入第一张图片
  基本概念: 1, // 在"基本概念"标题后插入第二张图片
  常用命令: 2, // 在"常用命令"标题后插入第三张图片
}

// 渲染带有图片的内容
const renderContentWithImages = () => {
  // 将Markdown转换为HTML
  let htmlContent = marked(note.value.content)

  // 为图片创建HTML
  const imageHtml = note.value.images.map((img) => {
    const positionClass = `image-${img.position}`
    return `
      <div class="note-image ${positionClass}">
        <img src="${img.url}" alt="${img.alt}" />
        <div class="image-caption">${img.alt}</div>
      </div>
    `
  })

  // 在特定位置插入图片
  // 这里使用简单的字符串替换方法，实际项目中可能需要更复杂的DOM操作
  Object.entries(imagePositions).forEach(([marker, imgIndex]) => {
    const img = note.value.images[imgIndex]
    if (img) {
      const regex = new RegExp(`<h\\d[^>]*>${marker}.*?</h\\d>`)
      const match = htmlContent.match(regex)
      if (match) {
        htmlContent = htmlContent.replace(match[0], `${match[0]}${imageHtml[imgIndex]}`)
      }
    }
  })

  return htmlContent
}

const addComment = () => {
  if (!newComment.value.content.trim()) return

  comments.value.push({
    id: comments.value.length + 1,
    content: newComment.value.content,
    author: '访客',
    avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${Math.random()}`,
    createTime: new Date().toLocaleString(),
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

    <div class="note-content markdown-body" v-html="renderContentWithImages()"></div>

    <div class="note-actions">
      <button class="like-button" :class="{ liked: note.isLiked }" @click="toggleLike">
        <i class="fas fa-heart"></i>
        <span>{{ note.likes }}</span>
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
.note-detail {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.note-title {
  font-size: 28px;
  color: #ffffff; /* 标题颜色改为白色 */
  margin-bottom: 16px;
}

.note-meta {
  margin-bottom: 24px;
  color: #a0a0a0; /* 更改为浅灰色，适合暗色主题 */
}

.note-content {
  line-height: 1.8;
  color: #ffffff; /* 内容颜色改为白色 */
  margin-bottom: 40px;
}

/* 图片样式 */
:deep(.note-image) {
  margin: 20px 0;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
}

:deep(.note-image img) {
  width: 100%;
  display: block;
}

:deep(.image-caption) {
  padding: 10px;
  background-color: rgba(0, 0, 0, 0.6);
  color: #ffffff;
  font-size: 0.9rem;
  text-align: center;
}

/* 图片位置样式 */
:deep(.image-left) {
  float: left;
  width: 45%;
  margin-right: 20px;
  margin-bottom: 20px;
}

:deep(.image-right) {
  float: right;
  width: 45%;
  margin-left: 20px;
  margin-bottom: 20px;
}

:deep(.image-center) {
  margin: 20px auto;
  width: 70%;
}

:deep(.image-full) {
  width: 100%;
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
  border-top: 1px solid #333333; /* 添加分隔线 */
  padding-top: 20px;
}

.comments-section h2 {
  color: #ffffff; /* 评论标题颜色 */
  margin-bottom: 20px;
}

.comment-form {
  margin-bottom: 24px;
}

textarea {
  width: 100%;
  padding: 12px;
  background-color: #2a2a2a; /* 输入框背景色 */
  color: #ffffff; /* 输入框文字颜色 */
  border: 1px solid #404040; /* 输入框边框 */
  border-radius: 8px;
  resize: vertical;
  margin-bottom: 12px;
  font-family: inherit;
}

textarea::placeholder {
  color: #a0a0a0; /* 占位符颜色 */
}

button {
  background: var(--color-primary); /* 使用主题蓝色 */
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.2s;
}

button:hover {
  background: var(--color-primary-dark); /* 深一点的主题蓝色 */
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
  background: #1e1e1e; /* 评论背景色 */
  border-radius: 8px;
  border: 1px solid #333333; /* 评论边框 */
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
  color: #ffffff; /* 作者名颜色 */
}

.time {
  font-size: 12px;
  color: #a0a0a0; /* 时间颜色 */
}

.text {
  margin: 0;
  color: #e0e0e0; /* 评论文字颜色 */
  line-height: 1.5;
}

/* 确保Markdown内容中的文字颜色也是白色 */
:deep(.markdown-body) {
  color: #ffffff;
}

:deep(.markdown-body h1),
:deep(.markdown-body h2),
:deep(.markdown-body h3),
:deep(.markdown-body h4),
:deep(.markdown-body h5),
:deep(.markdown-body h6) {
  color: #ffffff;
}

:deep(.markdown-body p),
:deep(.markdown-body li) {
  color: #e0e0e0;
}

:deep(.markdown-body code) {
  background-color: #2a2a2a;
  color: var(--color-primary);
}
</style>
