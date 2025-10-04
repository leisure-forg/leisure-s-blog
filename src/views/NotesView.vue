<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'

interface MomentImage {
  id: number
  url: string
}

interface MomentItem {
  id: number
  author: string
  avatar: string
  content: string
  location?: string
  time: string
  images: MomentImage[]
  likes: number
  liked: boolean
}

const moments = ref<MomentItem[]>([
  {
    id: 1,
    author: '周文章',
    avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Ada',
    content:
      '又一年刻度叠加，终于年满18，愿此后眼底有星光，行囊有远方，在时光里活成自己喜欢的模样。',
    location: '',
    time: '8小时前',
    images: [
      { id: 1, url: 'https://images.unsplash.com/photo-1519112232436-463994edeffb' },
    ],
    likes: 12,
    liked: false,
  },
  {
    id: 2,
    author: '国际创新研究院·陈慧敏',
    avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Grace',
    content: '祈的愿会圆',
    location: '苏州市·苏州市西园戒律寺',
    time: '11小时前',
    images: [
      { id: 1, url: 'https://images.unsplash.com/photo-1548584813-56c68bb06f57' },
      { id: 2, url: 'https://images.unsplash.com/photo-1483478550801-ceba5fe50e8e' },
      { id: 3, url: 'https://images.unsplash.com/photo-1500530855697-b586d89ba3ee' },
      { id: 4, url: 'https://images.unsplash.com/photo-1469474968028-56623f02e42e' },
      { id: 5, url: 'https://images.unsplash.com/photo-1494783367193-149034c05e8f' },
      { id: 6, url: 'https://images.unsplash.com/photo-1526318472351-c75aa4573afe' },
      { id: 7, url: 'https://images.unsplash.com/photo-1516574187841-8b8376e8d4b6' },
      { id: 8, url: 'https://images.unsplash.com/photo-1500048993953-d23a436266cf' },
      { id: 9, url: 'https://images.unsplash.com/photo-1446776811953-b23d57bd21aa' },
    ],
    likes: 36,
    liked: false,
  },
  {
    id: 3,
    author: '周文章',
    avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Bob',
    content: '同学小聚😄😄😄',
    location: '',
    time: '昨天',
    images: [
      { id: 1, url: 'https://images.unsplash.com/photo-1527090496-346715703c63' },
      { id: 2, url: 'https://images.unsplash.com/photo-1512621776951-a57141f2eefd' },
      { id: 3, url: 'https://images.unsplash.com/photo-1482049016688-2d3e1b311543' },
    ],
    likes: 20,
    liked: false,
  },
])

const toggleLike = (item: MomentItem) => {
  item.liked = !item.liked
  item.likes += item.liked ? 1 : -1
}

// 灯箱预览
const lightboxVisible = ref(false)
const currentImage = ref<MomentImage | null>(null)
const openLightbox = (img: MomentImage) => {
  currentImage.value = img
  lightboxVisible.value = true
}
const closeLightbox = () => {
  lightboxVisible.value = false
  currentImage.value = null
}
const onKeydown = (e: KeyboardEvent) => e.key === 'Escape' && closeLightbox()
onMounted(() => window.addEventListener('keydown', onKeydown))
onBeforeUnmount(() => window.removeEventListener('keydown', onKeydown))
</script>

<template>
  <div class="moments-page">
    <h1 class="page-title">朋友圈</h1>
    <div class="feed">
      <article v-for="item in moments" :key="item.id" class="moment-card">
        <header class="moment-header">
          <img class="avatar" :src="item.avatar" alt="avatar" />
          <div class="meta">
            <div class="author">{{ item.author }}</div>
            <div class="sub">{{ item.time }} <span v-if="item.location">· {{ item.location }}</span></div>
          </div>
        </header>

        <p class="content">{{ item.content }}</p>

        <div class="images" :class="`count-${item.images.length}`">
          <div v-for="img in item.images" :key="img.id" class="img-box" @click="openLightbox(img)">
            <img :src="img.url" alt="pic" />
          </div>
        </div>

        <footer class="actions">
          <button class="like" :class="{ liked: item.liked }" @click="toggleLike(item)">
            <i class="fas fa-heart"></i>
            <span>{{ item.likes }}</span>
          </button>
        </footer>
      </article>
    </div>

    <!-- 图片灯箱 -->
    <div v-if="lightboxVisible" class="lightbox" @click.self="closeLightbox">
      <img v-if="currentImage" :src="currentImage.url" alt="preview" />
      <button class="close" @click="closeLightbox">×</button>
    </div>
  </div>
</template>

<style scoped>
.moments-page { padding: 20px; }
.page-title { font-size: 22px; color: var(--color-text); margin-bottom: 16px; }

.feed { display: flex; flex-direction: column; gap: 16px; }
.moment-card { background: var(--color-background-soft); border-radius: 12px; padding: 12px; box-shadow: 0 2px 12px var(--color-shadow); }
.moment-header { display: flex; gap: 12px; align-items: center; }
.avatar { width: 40px; height: 40px; border-radius: 50%; object-fit: cover; }
.meta { display: flex; flex-direction: column; }
.author { font-weight: 600; color: var(--color-heading); }
.sub { color: var(--color-text-soft); font-size: 12px; }
.content { margin: 8px 0 12px 0; color: var(--color-text); line-height: 1.6; }

/* 九宫格图片布局，适配1-9张 */
.images { display: grid; gap: 6px; }
.images img { width: 100%; height: 100%; object-fit: cover; border-radius: 8px; display: block; }
.img-box { overflow: hidden; cursor: zoom-in; }
.images.count-1 { grid-template-columns: 1fr; }
.images.count-2 { grid-template-columns: repeat(2, 1fr); }
.images.count-3 { grid-template-columns: repeat(3, 1fr); }
.images.count-4 { grid-template-columns: repeat(2, 1fr); }
.images.count-5 { grid-template-columns: repeat(3, 1fr); }
.images.count-6 { grid-template-columns: repeat(3, 1fr); }
.images.count-7 { grid-template-columns: repeat(3, 1fr); }
.images.count-8 { grid-template-columns: repeat(3, 1fr); }
.images.count-9 { grid-template-columns: repeat(3, 1fr); }
.images.count-1 .img-box { height: 320px; }
.images.count-2 .img-box { height: 180px; }
.images.count-3 .img-box,
.images.count-4 .img-box,
.images.count-5 .img-box,
.images.count-6 .img-box,
.images.count-7 .img-box,
.images.count-8 .img-box,
.images.count-9 .img-box { height: 120px; }

.actions { margin-top: 8px; display: flex; justify-content: flex-end; }
.like { background: none; border: 1px solid #ff4757; color: #ff4757; padding: 4px 10px; border-radius: 16px; display: inline-flex; align-items: center; gap: 6px; cursor: pointer; }
.like:hover, .like.liked { background: #ff4757; color: #fff; }

/* 灯箱样式 */
.lightbox { position: fixed; inset: 0; background: rgba(0,0,0,.7); display: flex; align-items: center; justify-content: center; z-index: 9999; }
.lightbox img { max-width: 92vw; max-height: 82vh; border-radius: 8px; box-shadow: 0 8px 24px rgba(0,0,0,.4); }
.close { position: absolute; top: 16px; right: 20px; background: transparent; border: none; color: #fff; font-size: 26px; cursor: pointer; }

@media (max-width: 900px) {
  .images.count-1 .img-box { height: 240px; }
  .images.count-2 .img-box { height: 150px; }
  .images.count-3 .img-box { height: 100px; }
}
</style>
