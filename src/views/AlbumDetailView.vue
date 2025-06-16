<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'

interface Photo {
  id: number
  url: string
  description: string
}

interface Album {
  id: number
  title: string
  updateTime: string
  description: string
  photos: Photo[]
}

const route = useRoute()
const albumId = route.params.id

// 模拟相册数据
const albumsData = {
  '1': {
    id: 1,
    title: '春日游记',
    updateTime: '2025-05-20',
    description:
      '记录春天的美好时光，漫步在樱花纷飞的季节，感受大自然的生机与活力。每一张照片都记录着春天独特的魅力，定格下这美好的瞬间。',
    photos: [
      {
        id: 1,
        url: 'https://images.unsplash.com/photo-1506744038136-46273834b3fb',
        description: '春日樱花',
      },
      {
        id: 2,
        url: 'https://images.unsplash.com/photo-1506744476757-2fa06c5a3948',
        description: '春日野游',
      },
      {
        id: 3,
        url: 'https://images.unsplash.com/photo-1506744506831-911e3abd9354',
        description: '春日花海',
      },
    ],
  },
  '2': {
    id: 2,
    title: '夏日写真',
    updateTime: '2025-05-15',
    description:
      '夏天的缤纷色彩，海边的浪花，沙滩的脚印，还有那璀璨的阳光。每一帧都充满了夏天特有的活力与热情。',
    photos: [
      {
        id: 1,
        url: 'https://images.unsplash.com/photo-1506744476757-2fa06c5a3948',
        description: '夏日海滩',
      },
      {
        id: 2,
        url: 'https://images.unsplash.com/photo-1506744506831-911e3abd9354',
        description: '夏日晚霞',
      },
      {
        id: 3,
        url: 'https://images.unsplash.com/photo-1506744038136-46273834b3fb',
        description: '夏日森林',
      },
    ],
  },
}

const album = ref<Album | null>(null)

onMounted(() => {
  // 将 albumId 断言为 albumsData 对象的键类型
  const albumKey = albumId as keyof typeof albumsData
  const albumData = albumsData[albumKey]
  if (albumData) {
    album.value = albumData
  }
})
</script>

<template>
  <div v-if="album" class="album-detail">
    <header class="album-header">
      <h1 class="album-title">{{ album.title }}</h1>
      <p class="update-time">更新于：{{ album.updateTime }}</p>
      <p class="album-description">{{ album.description }}</p>
    </header>

    <div class="photos-grid">
      <div v-for="photo in album.photos" :key="photo.id" class="photo-card">
        <img :src="photo.url" :alt="photo.description" class="photo-image" />
        <p class="photo-description">{{ photo.description }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.album-detail {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.album-header {
  text-align: center;
  margin-bottom: 40px;
}

.album-title {
  font-size: 32px;
  color: #2c3e50;
  margin-bottom: 16px;
}

.update-time {
  color: #666;
  font-size: 14px;
  margin-bottom: 16px;
}

.album-description {
  color: #666;
  font-size: 16px;
  line-height: 1.6;
  max-width: 800px;
  margin: 0 auto;
}

.photos-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 24px;
}

.photo-card {
  background: #fff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
}

.photo-image {
  width: 100%;
  height: 250px;
  object-fit: cover;
}

.photo-description {
  padding: 12px;
  color: #666;
  font-size: 14px;
  text-align: center;
  margin: 0;
}

.error-message {
  text-align: center;
  padding: 40px;
}

.error-message i {
  font-size: 48px;
  color: #ff4757;
  margin-bottom: 16px;
}

.error-message p {
  color: #ff4757;
  font-size: 18px;
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
</style>
