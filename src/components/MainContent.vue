<template>
  <div class="main-content-area">
    <section class="recommend">
      <h2>推荐文章</h2>
      <div
        v-for="article in recommendedArticles"
        :key="article.id"
        class="article-card"
        @click="router.push(`/articles/${article.id}`)"
      >
        <div class="article-title">{{ article.title }}</div>
        <div class="article-meta">
          {{ article.updateTime }} |
          <span v-for="tag in article.tags" :key="tag">{{ tag }}</span>
        </div>
        <div class="article-desc">{{ article.description }}</div>
        <div class="article-actions">
          <span>👍 {{ article.likes }}</span>
          <span>💬 {{ article.comments }}</span>
          <span>👁️ {{ article.views }}</span>
        </div>
      </div>
    </section>

    <section class="recent-update">
      <h2>最近更新</h2>
      <div
        v-for="update in recentMonthUpdates"
        :key="update.id"
        class="update-card"
        @click="navigateToDetail(update)"
      >
        <img
          class="update-img"
          :src="update.imageUrl"
          :alt="update.title"
        />
        <div class="update-info">
          <div class="update-title">{{ update.title }}</div>
          <div class="update-meta">{{ update.updateTime }} | {{ update.category }}</div>
          <div class="update-desc">{{ update.description }}</div>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'

interface Article {
  id: number
  title: string
  updateTime: string
  description: string
  tags: string[]
  likes: number
  comments: number
  views: number
}

interface Update {
  id: number
  type: 'article' | 'note'
  title: string
  updateTime: string
  description: string
  category: string
  imageUrl: string
}

const router = useRouter()

// 推荐文章数据
const recommendedArticles = ref<Article[]>([
  {
    id: 1,
    title: 'Redis解决缓存击穿、缓存穿透方法',
    updateTime: '2025-05-09 15:53:59',
    description: '解决redis的缓存击穿和穿透问题是笔者的体会和理解...',
    tags: ['SpringBoot', 'Redis'],
    likes: 2,
    comments: 0,
    views: 85
  },
  {
    id: 2,
    title: 'Spring Security整合JWT实现认证授权',
    updateTime: '2025-05-08 14:30:00',
    description: '详细介绍如何在Spring Boot项目中整合Spring Security和JWT...',
    tags: ['Spring Security', 'JWT'],
    likes: 1,
    comments: 1,
    views: 56
  }
])

// 最近更新数据
const recentUpdates = ref<Update[]>([
  {
    id: 1,
    type: 'article',
    title: '聊一聊最近在干什么',
    updateTime: '2025-05-20 20:10:08',
    description: '最近技术又摸索了不少，聊聊最近在干的事情吧...',
    category: '最近状态',
    imageUrl: 'https://images.unsplash.com/photo-1506744038136-46273834b3fb'
  },
  {
    id: 2,
    type: 'note',
    title: '春日随笔',
    updateTime: '2025-05-19 15:30:00',
    description: '春天的气息扑面而来，记录下这美好的时刻...',
    category: '生活随笔',
    imageUrl: 'https://images.unsplash.com/photo-1506744476757-2fa06c5a3948'
  }
])

// 计算最近一个月的更新
const recentMonthUpdates = computed(() => {
  const oneMonthAgo = new Date()
  oneMonthAgo.setMonth(oneMonthAgo.getMonth() - 1)
  
  return recentUpdates.value.filter(update => {
    const updateDate = new Date(update.updateTime)
    return updateDate >= oneMonthAgo
  })
})

// 跳转到文章或随记详情
const navigateToDetail = (update: Update) => {
  if (update.type === 'article') {
    router.push(`/articles/${update.id}`)
  } else {
    router.push(`/notes/${update.id}`)
  }
}
</script>

<style scoped>
.main-content-area {
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  padding: 32px 24px;
  width: 100%;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
}

.weather-info {
  color: #333;
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 24px;
  padding: 16px 32px;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 16px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
  letter-spacing: 2px;
}

.recommend,
.recent-update {
  margin-bottom: 32px;
}

.article-card,
.update-card {
  background: #f8faff;
  border-radius: 12px;
  padding: 20px;
  margin-top: 12px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.12);
}

.article-title {
  font-weight: bold;
  font-size: 1.1rem;
  margin-bottom: 8px;
}

.article-meta {
  color: #888;
  font-size: 0.95rem;
  margin-bottom: 8px;
}

.article-meta span {
  background: #e3f0ff;
  color: #409eff;
  border-radius: 8px;
  padding: 2px 8px;
  margin-right: 6px;
  font-size: 0.9em;
}

.article-desc {
  color: #555;
  margin-bottom: 10px;
}

.article-actions span {
  margin-right: 16px;
  color: #888;
  font-size: 0.95em;
}

.update-card {
  display: flex;
  align-items: flex-start;
}

.update-img {
  width: 100px;
  height: 70px;
  object-fit: cover;
  border-radius: 8px;
  margin-right: 16px;
}

.update-info {
  flex: 1;
}

.update-title {
  font-weight: bold;
  margin-bottom: 6px;
}

.update-meta {
  color: #888;
  font-size: 0.95rem;
  margin-bottom: 6px;
}

.update-desc {
  color: #555;
}
</style>
