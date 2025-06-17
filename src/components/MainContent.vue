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
        <img class="update-img" :src="update.imageUrl" :alt="update.title" />
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
    views: 85,
  },
  {
    id: 2,
    title: 'Spring Security整合JWT实现认证授权',
    updateTime: '2025-05-08 14:30:00',
    description: '详细介绍如何在Spring Boot项目中整合Spring Security和JWT...',
    tags: ['Spring Security', 'JWT'],
    likes: 1,
    comments: 1,
    views: 56,
  },
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
    imageUrl: 'https://images.unsplash.com/photo-1506744038136-46273834b3fb',
  },
  {
    id: 2,
    type: 'note',
    title: '春日随笔',
    updateTime: '2025-05-19 15:30:00',
    description: '春天的气息扑面而来，记录下这美好的时刻...',
    category: '生活随笔',
    imageUrl: 'https://images.unsplash.com/photo-1506744476757-2fa06c5a3948',
  },
])

// 计算最近一个月的更新
const recentMonthUpdates = computed(() => {
  const oneMonthAgo = new Date()
  oneMonthAgo.setMonth(oneMonthAgo.getMonth() - 1)

  return recentUpdates.value.filter((update) => {
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
  background: var(--color-background-soft);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-md);
  padding: var(--space-xl);
  width: 100%;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  transition: box-shadow var(--transition-normal) var(--ease-out);
}

.main-content-area:hover {
  box-shadow: var(--shadow-lg);
}

.recommend,
.recent-update {
  margin-bottom: var(--space-2xl);
}

.recommend h2,
.recent-update h2 {
  color: var(--color-heading);
  font-size: var(--text-xl);
  font-weight: var(--font-bold);
  margin-bottom: var(--space-md);
  position: relative;
  padding-left: var(--space-md);
}

.recommend h2::before,
.recent-update h2::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 4px;
  height: 24px;
  background: var(--primary-500);
  border-radius: var(--radius-sm);
}

.article-card,
.update-card {
  background: var(--color-background);
  border-radius: var(--radius-md);
  padding: var(--space-lg);
  margin-top: var(--space-md);
  box-shadow: var(--shadow-sm);
  transition: all var(--transition-normal) var(--ease-out);
  cursor: pointer;
  border: 1px solid var(--color-border);
  position: relative;
  overflow: hidden;
}

.article-card::before,
.update-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 4px;
  height: 0;
  background: var(--primary-500);
  transition: height var(--transition-normal) var(--ease-out);
}

.article-card:hover,
.update-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
  border-color: var(--primary-200);
}

.article-card:hover::before,
.update-card:hover::before {
  height: 100%;
}

.article-title {
  font-weight: var(--font-bold);
  font-size: var(--text-lg);
  margin-bottom: var(--space-sm);
  color: var(--color-heading);
  transition: color var(--transition-normal) var(--ease-out);
}

.article-card:hover .article-title,
.update-card:hover .update-title {
  color: var(--primary-600);
}

.article-meta {
  color: var(--neutral-500);
  font-size: var(--text-sm);
  margin-bottom: var(--space-sm);
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: var(--space-xs);
}

.article-meta span {
  background: var(--primary-50);
  color: var(--primary-600);
  border-radius: var(--radius-full);
  padding: 2px var(--space-sm);
  font-size: var(--text-xs);
  font-weight: var(--font-medium);
  transition: all var(--transition-normal) var(--ease-out);
}

.article-meta span:hover {
  background: var(--primary-100);
  transform: scale(1.05);
}

.article-desc {
  color: var(--neutral-700);
  margin-bottom: var(--space-md);
  line-height: 1.6;
  font-size: var(--text-sm);
}

.article-actions {
  display: flex;
  align-items: center;
  gap: var(--space-lg);
}

.article-actions span {
  color: var(--neutral-500);
  font-size: var(--text-sm);
  display: flex;
  align-items: center;
  gap: var(--space-xs);
  transition: color var(--transition-normal) var(--ease-out);
}

.article-actions span:hover {
  color: var(--primary-500);
}

.update-card {
  display: flex;
  align-items: flex-start;
}

.update-img {
  width: 120px;
  height: 80px;
  object-fit: cover;
  border-radius: var(--radius-md);
  margin-right: var(--space-md);
  transition: all var(--transition-normal) var(--ease-out);
  box-shadow: var(--shadow-sm);
}

.update-card:hover .update-img {
  transform: scale(1.05);
  box-shadow: var(--shadow-md);
}

.update-info {
  flex: 1;
}

.update-title {
  font-weight: var(--font-bold);
  margin-bottom: var(--space-xs);
  color: var(--color-heading);
  font-size: var(--text-lg);
  transition: color var(--transition-normal) var(--ease-out);
}

.update-meta {
  color: var(--neutral-500);
  font-size: var(--text-sm);
  margin-bottom: var(--space-xs);
  display: flex;
  align-items: center;
  gap: var(--space-xs);
}

.update-desc {
  color: var(--neutral-700);
  line-height: 1.6;
  font-size: var(--text-sm);
}

/* 响应式设计 */
@media (max-width: 768px) {
  .main-content-area {
    padding: var(--space-md);
  }

  .update-card {
    flex-direction: column;
  }

  .update-img {
    width: 100%;
    height: 160px;
    margin-right: 0;
    margin-bottom: var(--space-md);
  }
}
</style>
