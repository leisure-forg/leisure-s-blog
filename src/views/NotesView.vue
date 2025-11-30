<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, reactive } from 'vue'

interface MomentImage {
  id: number
  url: string
}

interface CommentItem {
  id: number
  moment_id: number
  user_id: number
  content: string
  created_at: string
  parent_id: number | null
  nickname: string
  avatar_url: string
}

interface MomentItem {
  id: number
  author: string
  avatar: string
  content: string
  location?: string
  time: string
  images: MomentImage[]
  videos?: string[]
  likes: number
  liked: boolean
  comments: CommentItem[]
  showComments: boolean
  newComment: string
}

const moments = ref<MomentItem[]>([])
const loading = ref(false)
const showCreateModal = ref(false)
const submitting = ref(false)
const fileInput = ref<HTMLInputElement | null>(null)
const commentSubmitting = ref<number | null>(null)
const currentUser = ref<{
  id: number
  nickname?: string
  username?: string
  avatar_url?: string
} | null>(null)

const BASE_URL = 'http://139.196.162.210:8080'

const createForm = reactive({
  content: '',
  images: [] as string[],
  videos: [] as string[],
})

const getToken = () => localStorage.getItem('token')

const fetchUserInfo = async () => {
  const token = getToken()
  if (!token) return
  try {
    const response = await fetch(`${BASE_URL}/api/v1/users/me`, {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })
    if (response.ok) {
      currentUser.value = await response.json()
    }
  } catch (error) {
    console.error('Fetch user info error:', error)
  }
}

const fetchMoments = async () => {
  loading.value = true
  try {
    const token = getToken()
    const response = await fetch(`${BASE_URL}/api/v1/moments`, {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })
    if (response.ok) {
      const data = await response.json()
      // 适配后端返回的数据结构
      moments.value = data.map((item: any) => ({
        id: item.id,
        author: item.nickname || item.username || 'User', // 优先使用 nickname
        avatar:
          item.avatar_url ||
          item.avatar ||
          `https://api.dicebear.com/7.x/avataaars/svg?seed=${item.id}`, // 优先使用 avatar_url
        content: item.content,
        location: '', // 后端暂无 location
        time: item.created_at ? new Date(item.created_at).toLocaleString() : '',
        images: (item.images || []).map((url: string, index: number) => ({ id: index, url })),
        videos: item.videos || [],
        likes: item.like_count || 0,
        liked: false,
        comments: [],
        showComments: false,
        newComment: '',
      }))
    }
  } catch (error) {
    console.error('Fetch moments error:', error)
  } finally {
    loading.value = false
  }
}

const fetchComments = async (momentId: number) => {
  try {
    const token = getToken()
    const response = await fetch(`${BASE_URL}/api/v1/moments/${momentId}/comments`, {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })
    if (response.ok) {
      const data = await response.json()
      const moment = moments.value.find((m) => m.id === momentId)
      if (moment) {
        moment.comments = data
      }
    }
  } catch (error) {
    console.error('Fetch comments error:', error)
  }
}

const toggleComments = async (item: MomentItem) => {
  item.showComments = !item.showComments
  if (item.showComments && item.comments.length === 0) {
    await fetchComments(item.id)
  }
}

const handlePostComment = async (item: MomentItem) => {
  if (!item.newComment.trim()) {
    alert('请输入评论内容')
    return
  }

  commentSubmitting.value = item.id
  try {
    const token = getToken()

    if (!currentUser.value) {
      await fetchUserInfo()
    }

    const payload = {
      moment_id: item.id,
      user_id: currentUser.value?.id || 0,
      content: item.newComment,
      parent_id: null, // 暂不支持回复特定评论
      nickname: currentUser.value?.nickname || currentUser.value?.username || 'User',
      avatar_url: currentUser.value?.avatar_url,
    }

    const response = await fetch(`${BASE_URL}/api/v1/comments`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`,
      },
      body: JSON.stringify(payload),
    })

    if (response.ok) {
      item.newComment = ''
      await fetchComments(item.id)

      // 更新评论数量
      try {
        await fetch(`${BASE_URL}/api/v1/moments/${item.id}/counts`, {
          method: 'PATCH',
          headers: {
            'Content-Type': 'application/json',
            Authorization: `Bearer ${token}`,
          },
          body: JSON.stringify({
            like_delta: 0,
            comment_delta: 1,
          }),
        })
      } catch (e) {
        console.error('Failed to update comment count', e)
      }
    } else {
      alert('评论失败')
    }
  } catch (error) {
    console.error('Post comment error:', error)
    alert('评论出错')
  } finally {
    commentSubmitting.value = null
  }
}

const handleDelete = async (id: number) => {
  if (!confirm('确定要删除这条朋友圈吗？')) return

  try {
    const token = getToken()
    const response = await fetch(`${BASE_URL}/api/v1/moments/${id}`, {
      method: 'DELETE',
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })

    if (response.ok) {
      moments.value = moments.value.filter((m) => m.id !== id)
    } else {
      alert('删除失败')
    }
  } catch (error) {
    console.error('Delete error:', error)
    alert('请求出错')
  }
}

const triggerFileUpload = () => {
  fileInput.value?.click()
}

const generateUUID = () => {
  if (typeof crypto !== 'undefined' && crypto.randomUUID) {
    return crypto.randomUUID()
  }
  return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
    const r = (Math.random() * 16) | 0,
      v = c == 'x' ? r : (r & 0x3) | 0x8
    return v.toString(16)
  })
}

const handleFileUpload = async (event: Event) => {
  const target = event.target as HTMLInputElement
  if (!target.files || target.files.length === 0) return

  const file = target.files[0]
  const ext = file.name.split('.').pop() || 'jpg'
  const filename = `${generateUUID()}.${ext}`

  const formData = new FormData()
  formData.append('file', file)
  formData.append('folder', 'moments')
  formData.append('filename', filename)

  try {
    const token = getToken()
    const response = await fetch(`${BASE_URL}/api/v1/oss/upload`, {
      method: 'POST',
      headers: {
        Authorization: `Bearer ${token}`,
      },
      body: formData,
    })

    if (response.ok) {
      const result = await response.json()
      // 假设后端返回结构中包含 data 字段为 url，或者直接是 url
      const url = result.data || result.url || result

      if (['mp4', 'mov', 'avi'].includes(ext.toLowerCase())) {
        createForm.videos.push(url)
      } else {
        createForm.images.push(url)
      }
    } else {
      alert('上传失败')
    }
  } catch (error) {
    console.error('Upload error:', error)
    alert('上传出错')
  } finally {
    if (fileInput.value) fileInput.value.value = ''
  }
}

const removeImage = (index: number) => {
  createForm.images.splice(index, 1)
}

const removeVideo = (index: number) => {
  createForm.videos.splice(index, 1)
}

const handleCreate = async () => {
  if (!createForm.content && createForm.images.length === 0 && createForm.videos.length === 0) {
    alert('请输入内容或上传图片/视频')
    return
  }

  submitting.value = true
  try {
    const token = getToken()
    const payload = {
      content: createForm.content,
      images: createForm.images,
      videos: createForm.videos,
      like_count: 0,
      comment_count: 0,
    }

    const response = await fetch(`${BASE_URL}/api/v1/moments`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`,
      },
      body: JSON.stringify(payload),
    })

    if (response.ok) {
      showCreateModal.value = false
      createForm.content = ''
      createForm.images = []
      createForm.videos = []
      fetchMoments()
    } else {
      alert('发布失败')
    }
  } catch (error) {
    console.error('Create error:', error)
    alert('发布出错')
  } finally {
    submitting.value = false
  }
}

const toggleLike = async (item: MomentItem) => {
  try {
    const token = getToken()

    if (!currentUser.value) {
      await fetchUserInfo()
    }

    item.liked = !item.liked
    item.likes += item.liked ? 1 : -1

    const payload = {
      like_delta: item.liked ? 1 : -1,
      comment_delta: 0,
    }

    const response = await fetch(`${BASE_URL}/api/v1/moments/${item.id}/counts`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`,
      },
      body: JSON.stringify(payload),
    })

    if (!response.ok) {
      // 如果失败，回滚状态
      item.liked = !item.liked
      item.likes += item.liked ? 1 : -1
      alert('操作失败')
    }
  } catch (error) {
    console.error('Like error:', error)
    // 回滚状态
    item.liked = !item.liked
    item.likes += item.liked ? 1 : -1
  }
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

onMounted(() => {
  window.addEventListener('keydown', onKeydown)
  fetchUserInfo()
  fetchMoments()
})

onBeforeUnmount(() => window.removeEventListener('keydown', onKeydown))
</script>

<template>
  <div class="moments-page">
    <div class="page-header">
      <h1 class="page-title">朋友圈</h1>
      <button class="create-btn" @click="showCreateModal = true">发布动态</button>
    </div>

    <div class="feed">
      <div v-if="loading" class="loading">加载中...</div>

      <article v-for="item in moments" :key="item.id" class="moment-card">
        <header class="moment-header">
          <img class="avatar" :src="item.avatar" alt="avatar" />
          <div class="meta">
            <div class="author">{{ item.author }}</div>
            <div class="sub">
              {{ item.time }} <span v-if="item.location">· {{ item.location }}</span>
            </div>
          </div>
          <button class="delete-btn" @click="handleDelete(item.id)">删除</button>
        </header>

        <p class="content">{{ item.content }}</p>

        <!-- 视频展示 -->
        <div
          v-if="item.videos && item.videos.length > 0"
          class="videos"
          :class="`count-${item.videos.length}`"
        >
          <video
            v-for="(video, idx) in item.videos"
            :key="idx"
            :src="video"
            controls
            class="video-item"
          ></video>
        </div>

        <!-- 图片展示 -->
        <div
          v-if="item.images && item.images.length > 0"
          class="images"
          :class="`count-${item.images.length}`"
        >
          <div v-for="img in item.images" :key="img.id" class="img-box" @click="openLightbox(img)">
            <img :src="img.url" alt="pic" />
          </div>
        </div>

        <footer class="actions">
          <span class="time">{{ item.time }}</span>
          <button class="like-btn" :class="{ liked: item.liked }" @click="toggleLike(item)">
            <i class="fas fa-heart"></i>
            <span>{{ item.likes || '赞' }}</span>
          </button>
          <button class="comment-btn" @click="toggleComments(item)">
            <i class="fas fa-comment"></i>
            <span>{{ item.comments.length || '评论' }}</span>
          </button>
        </footer>

        <!-- 评论区 -->
        <div class="comments-section" v-if="item.showComments">
          <div class="likes-list" v-if="item.likes > 0">
            <i class="fas fa-heart"></i>
            <span>user1, user2, ...</span>
          </div>

          <div class="comments-list">
            <div v-for="comment in item.comments" :key="comment.id" class="comment-item">
              <span class="user">{{ comment.nickname || 'User' }}</span
              >:
              <span>{{ comment.content }}</span>
            </div>
            <div v-if="item.comments.length === 0" class="no-comments">暂无评论</div>
          </div>

          <form class="comment-input-box" @submit.prevent="handlePostComment(item)">
            <input v-model="item.newComment" type="text" placeholder="说点什么..." />
            <button type="submit" :disabled="commentSubmitting === item.id">发送</button>
          </form>
        </div>
      </article>
    </div>

    <!-- 发布弹窗 -->
    <div v-if="showCreateModal" class="modal-overlay" @click.self="showCreateModal = false">
      <div class="modal-content">
        <h3 class="modal-title">发布朋友圈</h3>
        <textarea
          v-model="createForm.content"
          placeholder="这一刻的想法..."
          rows="4"
          class="modal-input"
        ></textarea>

        <div class="upload-section">
          <div class="preview-list">
            <div v-for="(img, idx) in createForm.images" :key="'img' + idx" class="preview-item">
              <img :src="img" />
              <div class="remove-btn" @click.stop="removeImage(idx)">×</div>
            </div>
            <div
              v-for="(vid, idx) in createForm.videos"
              :key="'vid' + idx"
              class="preview-item video"
            >
              <video :src="vid"></video>
              <div class="remove-btn" @click.stop="removeVideo(idx)">×</div>
            </div>
            <div class="upload-btn" @click="triggerFileUpload">
              <span>+</span>
            </div>
          </div>
          <input
            type="file"
            ref="fileInput"
            @change="handleFileUpload"
            style="display: none"
            accept="image/*,video/*"
          />
        </div>

        <div class="modal-actions">
          <button class="cancel-btn" @click="showCreateModal = false">取消</button>
          <button class="post-btn" @click="handleCreate" :disabled="submitting">
            {{ submitting ? '发布中...' : '发布' }}
          </button>
        </div>
      </div>
    </div>

    <!-- 图片灯箱 -->
    <div v-if="lightboxVisible" class="lightbox" @click.self="closeLightbox">
      <img v-if="currentImage" :src="currentImage.url" alt="preview" />
      <button class="close" @click="closeLightbox">×</button>
    </div>
  </div>
</template>

<style scoped>
.moments-page {
  padding: 20px;
}
.page-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}
.page-title {
  font-size: 22px;
  color: var(--color-text);
  margin: 0;
}
.create-btn {
  background: #ff6b35;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 20px;
  cursor: pointer;
}

.feed {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.moment-card {
  background: var(--color-background-soft);
  border-radius: 12px;
  padding: 12px;
  box-shadow: 0 2px 12px var(--color-shadow);
}
.moment-header {
  display: flex;
  gap: 12px;
  align-items: center;
  position: relative;
}
.delete-btn {
  margin-left: auto;
  background: none;
  border: none;
  color: #999;
  cursor: pointer;
  font-size: 12px;
}
.delete-btn:hover {
  color: #ff4757;
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  object-fit: cover;
}
.meta {
  display: flex;
  flex-direction: column;
}
.author {
  font-weight: 600;
  color: var(--color-heading);
}
.sub {
  color: var(--color-text-soft);
  font-size: 12px;
}
.content {
  margin: 8px 0 12px 0;
  color: var(--color-text);
  line-height: 1.6;
}

/* 视频样式 */
.videos {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 8px;
}
.video-item {
  max-width: 100%;
  max-height: 300px;
  border-radius: 4px;
  background: #000;
}

/* 九宫格图片布局 */
.images {
  display: grid;
  gap: 6px;
  margin-top: 8px;
  width: fit-content;
  max-width: 100%;
}
.images img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 4px;
  display: block;
}
.img-box {
  overflow: hidden;
  cursor: zoom-in;
  position: relative;
  padding-top: 100%; /* 1:1 aspect ratio */
}
.img-box img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

/* 单张图片特殊处理 */
.images.count-1 {
  grid-template-columns: 1fr;
  max-width: 240px; /* 限制单张图片最大宽度 */
}
.images.count-1 .img-box {
  padding-top: 0; /* 移除固定比例 */
  height: auto;
  max-height: 320px;
}
.images.count-1 .img-box img {
  position: static;
  height: auto;
  max-height: 320px;
}

/* 2张和4张图片：2列 */
.images.count-2,
.images.count-4 {
  grid-template-columns: repeat(2, 1fr);
  max-width: 240px; /* 约 110px * 2 + gap */
}

/* 3, 5-9张图片：3列 */
.images.count-3,
.images.count-5,
.images.count-6,
.images.count-7,
.images.count-8,
.images.count-9 {
  grid-template-columns: repeat(3, 1fr);
  max-width: 360px; /* 约 110px * 3 + gap */
}

.img-box {
  width: 110px; /* 固定单元格宽度，模仿微信 */
  height: 110px;
  padding-top: 0; /* 使用固定高度 */
}
/* 覆盖上面的 padding-top: 100% 逻辑，直接用固定宽高更接近微信效果 */

/* 响应式调整 */
@media (max-width: 400px) {
  .images.count-2,
  .images.count-4 {
    max-width: 200px;
  }
  .images.count-3,
  .images.count-5,
  .images.count-6,
  .images.count-7,
  .images.count-8,
  .images.count-9 {
    max-width: 300px;
  }
  .img-box {
    width: 90px;
    height: 90px;
  }
}

.actions {
  margin-top: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
}
.time {
  color: var(--color-text-soft);
  font-size: 12px;
  margin-right: auto;
}

.like-btn,
.comment-btn {
  background: transparent;
  border: none;
  color: var(--color-text-soft);
  padding: 4px 8px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 4px;
}
.like-btn:hover,
.comment-btn:hover {
  background: var(--color-background-soft);
}
.like-btn.liked {
  color: #ff4757;
}

/* 评论区样式 */
.comments-section {
  margin-top: 8px;
  background: var(--color-background-soft);
  border-radius: 4px;
  padding: 6px 10px;
  font-size: 13px;
  position: relative;
}
/* 小三角 */
.comments-section::before {
  content: '';
  position: absolute;
  top: -6px;
  left: 12px;
  width: 0;
  height: 0;
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-bottom: 6px solid var(--color-background-soft);
}

.likes-list {
  color: #576b95;
  border-bottom: 1px solid var(--color-border);
  padding-bottom: 6px;
  margin-bottom: 6px;
}
.likes-list i {
  margin-right: 4px;
  font-size: 12px;
}

.comments-list {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.comment-item {
  color: var(--color-text);
}
.comment-item .user {
  color: #576b95;
  font-weight: 500;
  cursor: pointer;
}

.no-comments {
  color: var(--color-text-soft);
  font-size: 12px;
  padding: 4px 0;
}

.comment-input-box {
  display: flex;
  gap: 8px;
  margin-top: 8px;
  padding-top: 8px;
  border-top: 1px solid var(--color-border);
}

.comment-input-box input {
  flex: 1;
  padding: 6px 10px;
  border: 1px solid var(--color-border);
  border-radius: 4px;
  background: var(--color-background);
  color: var(--color-text);
  font-size: 13px;
}

.comment-input-box input:focus {
  outline: none;
  border-color: var(--color-primary);
}

.comment-input-box button {
  padding: 6px 12px;
  background: var(--color-primary);
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 12px;
  cursor: pointer;
}

.comment-input-box button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

/* 弹窗样式更新 */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  backdrop-filter: blur(2px);
}
.modal-content {
  background: var(--color-background);
  color: var(--color-text);
  padding: 24px;
  border-radius: 12px;
  width: 90%;
  max-width: 500px;
  max-height: 85vh; /* 限制最大高度 */
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  transition: all 0.3s ease;
}
.modal-title {
  margin-top: 0;
  margin-bottom: 16px;
  font-size: 18px;
  font-weight: 600;
  color: var(--color-heading);
  flex-shrink: 0; /* 标题不压缩 */
}
.modal-input {
  width: 100%;
  padding: 12px;
  border: 1px solid var(--color-border);
  border-radius: 8px;
  resize: none;
  margin-bottom: 16px;
  background: var(--color-background-soft);
  color: var(--color-text);
  font-family: inherit;
  transition: border-color 0.2s;
  flex-shrink: 0; /* 输入框不压缩 */
}
.modal-input:focus {
  outline: none;
  border-color: var(--color-primary);
}

.upload-section {
  margin-bottom: 24px;
  overflow-y: auto; /* 上传区域内容过多时滚动 */
  flex-grow: 1; /* 占据剩余空间 */
  min-height: 100px; /* 最小高度 */
}
.preview-list {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}
.preview-item {
  width: 80px;
  height: 80px;
  border-radius: 4px;
  overflow: hidden;
  flex-shrink: 0;
  position: relative;
}
.remove-btn {
  position: absolute;
  top: 0;
  right: 0;
  width: 20px;
  height: 20px;
  background: rgba(0, 0, 0, 0.5);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 18px;
  line-height: 1;
  border-bottom-left-radius: 4px;
  transition: background 0.2s;
}
.remove-btn:hover {
  background: rgba(0, 0, 0, 0.7);
}
.preview-item img,
.preview-item video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.upload-btn {
  width: 80px;
  height: 80px;
  border: 1px dashed var(--color-border);
  background: var(--color-background-soft);
  color: var(--color-text-soft);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  border-radius: 4px;
  font-size: 24px;
  transition: all 0.2s;
  flex-shrink: 0;
}
.upload-btn:hover {
  border-color: var(--color-text-soft);
  background: var(--color-background-mute);
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  flex-shrink: 0; /* 按钮区域不压缩 */
  padding-top: 16px;
  border-top: 1px solid var(--color-border); /* 分隔线 */
}
.cancel-btn {
  padding: 8px 20px;
  border-radius: 6px;
  border: 1px solid var(--color-border);
  background: transparent;
  color: var(--color-text);
  cursor: pointer;
  transition: background 0.2s;
}
.cancel-btn:hover {
  background: var(--color-background-soft);
}
.post-btn {
  padding: 8px 20px;
  border-radius: 6px;
  border: none;
  background: var(--success);
  color: white;
  cursor: pointer;
  font-weight: 500;
  transition: opacity 0.2s;
}
.post-btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}
.post-btn:hover:not(:disabled) {
  opacity: 0.9;
}
</style>
