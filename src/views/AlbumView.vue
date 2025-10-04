<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'

interface Photo {
  id: number
  url: string
  caption?: string
}

interface AlbumDay {
  date: string
  photos: Photo[]
}

// 时间线数据示例
const albumTimeline = ref<AlbumDay[]>([
  {
    date: '2025-06-27',
    photos: [
      { id: 1, url: 'https://images.unsplash.com/photo-1500530855697-b586d89ba3ee', caption: '夜空' },
      { id: 2, url: 'https://images.unsplash.com/photo-1516574187841-8b8376e8d4b6', caption: '屏幕记录' },
    ],
  },
  {
    date: '2025-06-23',
    photos: [
      { id: 3, url: 'https://images.unsplash.com/photo-1501973801540-537f08ccae7b', caption: '日落余晖' },
      { id: 4, url: 'https://images.unsplash.com/photo-1469474968028-56623f02e42e', caption: '城市黄昏' },
      { id: 5, url: 'https://images.unsplash.com/photo-1494783367193-149034c05e8f', caption: '天际线' },
    ],
  },
  {
    date: '2025-06-06',
    photos: [
      { id: 6, url: 'https://images.unsplash.com/photo-1502082553048-f009c37129b9', caption: '夜色与月' },
      { id: 7, url: 'https://images.unsplash.com/photo-1446776811953-b23d57bd21aa', caption: '云涌风起' },
      { id: 8, url: 'https://images.unsplash.com/photo-1500530855697-b586d89ba3ee', caption: '道路与天空' },
    ],
  },
  {
    date: '2025-06-02',
    photos: [
      { id: 9, url: 'https://images.unsplash.com/photo-1500048993953-d23a436266cf', caption: '清晨林荫' },
      { id: 10, url: 'https://images.unsplash.com/photo-1496386261019-65bb2247c6d7', caption: '灯光与操场' },
    ],
  },
])

// 轻量灯箱
const lightboxVisible = ref(false)
const currentImage = ref<Photo | null>(null)

const openLightbox = (photo: Photo) => {
  currentImage.value = photo
  lightboxVisible.value = true
}

const closeLightbox = () => {
  lightboxVisible.value = false
  currentImage.value = null
}

const onKeydown = (e: KeyboardEvent) => {
  if (e.key === 'Escape') closeLightbox()
}

onMounted(() => {
  window.addEventListener('keydown', onKeydown)
})

onBeforeUnmount(() => {
  window.removeEventListener('keydown', onKeydown)
})
</script>

<template>
  <div class="album-page">
    <h1 class="title">相册</h1>
    <p class="subtitle">每一个精彩的瞬间都值得被记录</p>

    <div class="timeline">
      <div class="line"></div>

      <section v-for="day in albumTimeline" :key="day.date" class="timeline-item">
        <div class="marker">
          <span class="dot"></span>
        </div>
        <div class="content">
          <h2 class="date">{{ day.date }}</h2>
          <div class="photos">
            <div v-for="photo in day.photos" :key="photo.id" class="photo-card" @click="openLightbox(photo)">
              <img :src="photo.url" :alt="photo.caption || 'photo'" class="photo" />
            </div>
          </div>
        </div>
      </section>
    </div>

    <!-- 灯箱预览 -->
    <div v-if="lightboxVisible" class="lightbox" @click.self="closeLightbox">
      <img v-if="currentImage" :src="currentImage.url" :alt="currentImage.caption || 'preview'" />
      <button class="close" @click="closeLightbox" aria-label="关闭">×</button>
      <p v-if="currentImage?.caption" class="caption">{{ currentImage.caption }}</p>
    </div>
  </div>
  
</template>

<style scoped>
.album-page {
  padding: 24px;
}

.title {
  font-size: 28px;
  color: var(--color-heading);
  margin: 0 0 4px 0;
}

.subtitle {
  color: var(--color-text-soft);
  margin: 0 0 24px 0;
}

.timeline {
  position: relative;
  max-width: 1000px;
}

.line {
  position: absolute;
  left: 12px;
  top: 0;
  bottom: 0;
  width: 2px;
  background: var(--color-border);
}

.timeline-item {
  display: grid;
  grid-template-columns: 40px 1fr;
  gap: 16px;
  margin-bottom: 28px;
}

.marker {
  position: relative;
}

.dot {
  display: inline-block;
  width: 12px;
  height: 12px;
  background: var(--color-accent, #4a90e2);
  border-radius: 50%;
  position: relative;
  top: 2px;
  left: 6px;
  box-shadow: 0 0 0 4px rgba(74, 144, 226, 0.15);
}

.content {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.date {
  margin: 0;
  color: var(--color-text);
  font-weight: 600;
}

.photos {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
  gap: 16px;
}

.photo-card {
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  background: var(--color-background-soft);
  cursor: zoom-in;
}

.photo {
  width: 100%;
  height: 180px;
  object-fit: cover;
  display: block;
}

/* 灯箱样式 */
.lightbox {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
}

.lightbox img {
  max-width: 90vw;
  max-height: 80vh;
  border-radius: 8px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.4);
}

.close {
  position: absolute;
  top: 20px;
  right: 24px;
  background: transparent;
  border: none;
  color: #fff;
  font-size: 28px;
  cursor: pointer;
}

.caption {
  position: absolute;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%);
  color: #fff;
  background: rgba(0, 0, 0, 0.4);
  padding: 8px 12px;
  border-radius: 6px;
}

@media (max-width: 900px) {
  .photo { height: 140px; }
}
</style>