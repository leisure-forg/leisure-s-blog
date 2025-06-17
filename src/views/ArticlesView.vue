<script setup lang="ts">
import { ref } from 'vue'

interface Article {
  id: number
  title: string
  updateTime: string
  description: string
  tags: string[]
}

const articles = ref<Article[]>([
  {
    id: 1,
    title: 'Redis解决缓存击穿、缓存穿透方法',
    updateTime: '2025-05-09 15:53:59',
    description: '解决redis的缓存击穿和穿透问题是笔者的体会和理解...',
    tags: ['SpringBoot', 'Redis'],
  },
  {
    id: 2,
    title: 'Spring Security整合JWT实现认证授权',
    updateTime: '2025-05-08 14:30:00',
    description: '详细介绍如何在Spring Boot项目中整合Spring Security和JWT...',
    tags: ['Spring Security', 'JWT'],
  },
])
</script>

<template>
  <div class="articles-container">
    <h1 class="page-title">文章列表</h1>
    <div class="articles-list">
      <div
        v-for="article in articles"
        :key="article.id"
        class="article-box"
        @click="$router.push(`/articles/${article.id}`)"
      >
        <h2 class="article-title">{{ article.title }}</h2>
        <div class="article-meta">
          <span class="update-time">{{ article.updateTime }}</span>
          <span v-for="tag in article.tags" :key="tag" class="tag">{{ tag }}</span>
        </div>
        <p class="article-description">{{ article.description }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.articles-container {
  padding: 20px;
}

.page-title {
  font-size: 24px;
  margin-bottom: 20px;
  color: var(--color-text);
}

.articles-list {
  display: grid;
  gap: 20px;
}

.article-box {
  background: var(--color-background-soft);
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 12px var(--color-shadow);
  transition: transform 0.2s ease;
  cursor: pointer;
}

.article-box:hover {
  transform: translateY(-2px);
}

.article-title {
  font-size: 18px;
  color: var(--color-text);
  margin: 0 0 10px 0;
}

.article-meta {
  margin-bottom: 10px;
  font-size: 14px;
  color: var(--color-text-soft);
}

.update-time {
  margin-right: 15px;
}

.tag {
  background: var(--color-background-mute);
  padding: 2px 8px;
  border-radius: 4px;
  margin-right: 8px;
  font-size: 12px;
  color: var(--color-text-soft);
}

.article-description {
  color: var(--color-text-soft);
  font-size: 14px;
  margin: 0;
  line-height: 1.5;
}
</style>
