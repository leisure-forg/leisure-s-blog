<script setup lang="ts">
import { ref } from 'vue'

interface Album {
  id: number
  title: string
  updateTime: string
  description: string
  coverImages: string[]
}

const albums = ref<Album[]>([
  {
    id: 1,
    title: '春日游记',
    updateTime: '2025-05-20',
    description: '记录春天的美好时光...',
    coverImages: [
      'https://images.unsplash.com/photo-1506744038136-46273834b3fb',
      'https://images.unsplash.com/photo-1506744476757-2fa06c5a3948',
      'https://images.unsplash.com/photo-1506744506831-911e3abd9354',
    ],
  },
  {
    id: 2,
    title: '夏日写真',
    updateTime: '2025-05-15',
    description: '夏天的缤纷色彩...',
    coverImages: [
      'https://images.unsplash.com/photo-1506744476757-2fa06c5a3948',
      'https://images.unsplash.com/photo-1506744506831-911e3abd9354',
      'https://images.unsplash.com/photo-1506744038136-46273834b3fb',
    ],
  },
])
</script>

<template>
  <div class="albums-container">
    <h1 class="page-title">相册</h1>
    <div class="albums-grid">
      <div
        v-for="album in albums"
        :key="album.id"
        class="album-box"
        @click="$router.push(`/albums/${album.id}`)"
      >
        <div class="images-preview">
          <img
            v-for="(image, index) in album.coverImages.slice(0, 3)"
            :key="index"
            :src="image"
            :alt="album.title"
            class="preview-image"
          />
        </div>
        <div class="album-info">
          <h2 class="album-title">{{ album.title }}</h2>
          <p class="update-time">{{ album.updateTime }}</p>
          <p class="album-description">{{ album.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.albums-container {
  padding: 20px;
}

.page-title {
  font-size: 24px;
  margin-bottom: 20px;
  color: var(--color-text);
}

.albums-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}

.album-box {
  background: var(--color-background-soft);
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 12px var(--color-shadow);
  cursor: pointer;
  transition: transform 0.2s ease;
}

.album-box:hover {
  transform: translateY(-2px);
}

.images-preview {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 4px;
  padding: 4px;
}

.preview-image {
  width: 100%;
  height: 100px;
  object-fit: cover;
  border-radius: 4px;
}

.album-info {
  padding: 15px;
}

.album-title {
  font-size: 18px;
  color: var(--color-text);
  margin: 0 0 8px 0;
}

.update-time {
  color: var(--color-text-soft);
  font-size: 14px;
  margin-bottom: 8px;
}

.album-description {
  color: var(--color-text-soft);
  font-size: 14px;
  margin: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
</style>
