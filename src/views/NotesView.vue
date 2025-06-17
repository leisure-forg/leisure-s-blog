<script setup lang="ts">
import { ref } from 'vue'

interface Note {
  id: number
  title: string
  updateTime: string
  description: string
  imageUrl: string
}

const notes = ref<Note[]>([
  {
    id: 1,
    title: '聊一聊最近在干什么',
    updateTime: '2025-05-20 20:10:08',
    description: '最近技术又摸索了不少，聊聊最近在干的事情吧...',
    imageUrl:
      'https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=400&q=80',
  },
  {
    id: 2,
    title: '记录一下学习Docker的心得',
    updateTime: '2025-05-15 16:20:00',
    description: '这周开始系统学习Docker，分享一下使用心得和注意事项...',
    imageUrl:
      'https://images.unsplash.com/photo-1605745341112-85968b19335b?auto=format&fit=crop&w=400&q=80',
  },
])
</script>

<template>
  <div class="notes-container">
    <h1 class="page-title">随记</h1>
    <div class="notes-grid">
      <div
        v-for="note in notes"
        :key="note.id"
        class="note-box"
        @click="$router.push(`/notes/${note.id}`)"
      >
        <div class="note-image">
          <img :src="note.imageUrl" :alt="note.title" />
        </div>
        <div class="note-content">
          <h2 class="note-title">{{ note.title }}</h2>
          <div class="note-meta">{{ note.updateTime }}</div>
          <p class="note-description">{{ note.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.notes-container {
  padding: 20px;
}

.page-title {
  font-size: 24px;
  margin-bottom: 20px;
  color: var(--color-text);
}

.notes-grid {
  display: grid;
  gap: 24px;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
}

.note-box {
  background: var(--color-background-soft);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 12px var(--color-shadow);
  transition: transform 0.2s ease;
  cursor: pointer;
}

.note-box:hover {
  transform: translateY(-4px);
}

.note-image {
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.note-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.note-box:hover .note-image img {
  transform: scale(1.05);
}

.note-content {
  padding: 16px;
}

.note-title {
  font-size: 18px;
  color: var(--color-text);
  margin: 0 0 8px 0;
}

.note-meta {
  font-size: 14px;
  color: var(--color-text-soft);
  margin-bottom: 8px;
}

.note-description {
  color: var(--color-text-soft);
  font-size: 14px;
  margin: 0;
  line-height: 1.5;
}
</style>
