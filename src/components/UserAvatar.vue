<template>
  <div class="user-avatar-wrapper">
    <div class="user-avatar" @click.stop="toggleDropdown">
      <img :src="userAvatar" alt="用户头像" class="avatar-img" />
      <div class="dropdown-menu" v-show="isDropdownVisible" @click.stop>
        <div class="user-info">
          <span class="username">{{ username }}</span>
          <span class="email">{{ email }}</span>
        </div>
        <div class="menu-items">
          <a href="#" class="menu-item" @click.prevent="openProfileModal">
            <span class="icon">👤</span>
            个人资料
          </a>
          <a href="#" class="menu-item" @click.prevent="handleLogout">
            <span class="icon">🚪</span>
            退出登录
          </a>
        </div>
      </div>
    </div>

    <!-- 个人资料编辑模态框 -->
    <Teleport to="body">
      <div v-if="showProfileModal" class="modal-overlay" @click.self="closeProfileModal">
        <div class="modal-content">
          <div class="modal-header">
            <h3>编辑个人资料</h3>
            <button class="close-btn" @click="closeProfileModal">×</button>
          </div>

          <div class="modal-body">
            <div class="avatar-upload-section">
              <div class="avatar-preview" @click="triggerFileInput">
                <img :src="tempAvatar || userAvatar" alt="头像预览" />
                <div class="avatar-overlay">
                  <i class="fas fa-camera"></i>
                  <span>更换头像</span>
                </div>
              </div>
              <input
                type="file"
                ref="fileInput"
                accept="image/*"
                style="display: none"
                @change="handleFileChange"
              />
            </div>

            <div class="form-group">
              <label>用户名 (不可修改)</label>
              <input type="text" :value="username" disabled class="form-input disabled" />
            </div>

            <div class="form-group">
              <label>邮箱 (不可修改)</label>
              <input type="text" :value="email" disabled class="form-input disabled" />
            </div>

            <div class="form-group">
              <label>个性签名</label>
              <textarea
                v-model="bio"
                rows="3"
                placeholder="介绍一下你自己..."
                class="form-input"
              ></textarea>
            </div>
          </div>

          <div class="modal-footer">
            <button class="btn-cancel" @click="closeProfileModal">取消</button>
            <button class="btn-save" @click="handleSaveProfile" :disabled="isSaving">
              {{ isSaving ? '保存中...' : '保存' }}
            </button>
          </div>
        </div>
      </div>
    </Teleport>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const BASE_URL = 'http://139.196.162.210:8080'

// 状态
const isDropdownVisible = ref(false)
const showProfileModal = ref(false)
const isSaving = ref(false)
const fileInput = ref<HTMLInputElement | null>(null)

// 用户数据
const username = ref('')
const email = ref('')
const userAvatar = ref('https://api.dicebear.com/7.x/avataaars/svg?seed=default')
const bio = ref('')
const tempAvatar = ref('') // 用于预览上传的头像

// 获取 Token
const getToken = () => localStorage.getItem('token')

// 获取用户信息
const fetchUserInfo = async () => {
  try {
    const token = getToken()
    if (!token) return

    const response = await fetch(`${BASE_URL}/api/v1/users/me`, {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })

    if (response.ok) {
      const data = await response.json()
      // 假设后端返回的数据结构
      username.value = data.username || data.name || 'User'
      email.value = data.email || ''
      // 优先使用 avatar_url
      userAvatar.value =
        data.avatar_url ||
        data.avatar ||
        `https://api.dicebear.com/7.x/avataaars/svg?seed=${data.id}`
      bio.value = data.bio || ''
    }
  } catch (error) {
    console.error('Fetch user info error:', error)
  }
}

// 生成 UUID 的简单辅助函数
const generateUUID = () => {
  if (typeof crypto !== 'undefined' && crypto.randomUUID) {
    return crypto.randomUUID()
  }
  return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
    const r = (Math.random() * 16) | 0
    const v = c === 'x' ? r : (r & 0x3) | 0x8
    return v.toString(16)
  })
}

// 下拉菜单控制
const toggleDropdown = () => {
  isDropdownVisible.value = !isDropdownVisible.value
}

const closeDropdown = () => {
  isDropdownVisible.value = false
}

// 点击外部关闭下拉菜单
const handleClickOutside = (event: MouseEvent) => {
  const target = event.target as HTMLElement
  if (!target.closest('.user-avatar-wrapper')) {
    closeDropdown()
  }
}

// 模态框控制
const openProfileModal = () => {
  closeDropdown()
  tempAvatar.value = '' // 重置临时头像
  showProfileModal.value = true
  // 重新获取最新信息（可选）
  fetchUserInfo()
}

const closeProfileModal = () => {
  showProfileModal.value = false
}

// 登出
const handleLogout = () => {
  localStorage.removeItem('isLoggedIn')
  localStorage.removeItem('token')
  router.push('/login')
}

// 头像上传逻辑
const triggerFileInput = () => {
  fileInput.value?.click()
}

const handleFileChange = async (event: Event) => {
  const target = event.target as HTMLInputElement
  if (!target.files || target.files.length === 0) return

  const file = target.files[0]
  // 本地预览
  tempAvatar.value = URL.createObjectURL(file)

  // 上传头像
  try {
    const token = getToken()
    const formData = new FormData()
    formData.append('file', file)
    formData.append('folder', 'users') // 指定 folder 为 users

    // 生成 UUID 文件名并保留后缀
    const extension = file.name.split('.').pop()
    const filename = `${generateUUID()}.${extension}`
    formData.append('filename', filename)

    // 1. 上传到 OSS
    const uploadRes = await fetch(`${BASE_URL}/api/v1/oss/upload`, {
      method: 'POST',
      headers: {
        Authorization: `Bearer ${token}`,
      },
      body: formData,
    })

    if (!uploadRes.ok) throw new Error('Upload failed')

    const uploadData = await uploadRes.json()
    const newAvatarUrl = uploadData.data || uploadData.url

    // 2. 更新用户头像信息
    const updateRes = await fetch(`${BASE_URL}/api/v1/users/me/avatar`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`,
      },
      body: JSON.stringify({ avatar_url: newAvatarUrl }),
    })

    if (updateRes.ok) {
      userAvatar.value = newAvatarUrl
      alert('头像更新成功')
    } else {
      throw new Error('Update avatar failed')
    }
  } catch (error) {
    console.error('Avatar update error:', error)
    alert('头像更新失败')
    tempAvatar.value = '' // 恢复原样
  } finally {
    if (fileInput.value) fileInput.value.value = ''
  }
}

// 保存资料（Bio）
const handleSaveProfile = async () => {
  isSaving.value = true
  try {
    const token = getToken()

    // 更新 Bio
    const response = await fetch(`${BASE_URL}/api/v1/users/me/bio`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`,
      },
      body: JSON.stringify({ bio: bio.value }),
    })

    if (response.ok) {
      alert('资料保存成功')
      closeProfileModal()
      fetchUserInfo() // 刷新数据
    } else {
      alert('保存失败')
    }
  } catch (error) {
    console.error('Save profile error:', error)
    alert('保存出错')
  } finally {
    isSaving.value = false
  }
}

onMounted(() => {
  fetchUserInfo()
  window.addEventListener('click', handleClickOutside)
})

onBeforeUnmount(() => {
  window.removeEventListener('click', handleClickOutside)
})
</script>

<style scoped>
.user-avatar-wrapper {
  position: relative;
}

.user-avatar {
  position: relative;
  cursor: pointer;
  z-index: 100; /* 保证头像在顶层 */
}

.avatar-img {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 2px solid var(--color-primary);
  background: var(--color-background);
  box-shadow: var(--shadow-sm);
  object-fit: cover;
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  right: 0;
  width: 220px;
  background: var(--color-background);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  box-shadow: var(--shadow-md);
  margin-top: 12px;
  z-index: 101;
  overflow: hidden;
}

.user-info {
  padding: 16px;
  border-bottom: 1px solid var(--color-border);
  background: var(--color-background-soft);
}

.username {
  display: block;
  font-weight: bold;
  color: var(--color-heading);
  font-size: 16px;
}

.email {
  display: block;
  font-size: 12px;
  color: var(--color-text-soft);
  margin-top: 4px;
  word-break: break-all;
}

.menu-items {
  padding: 8px 0;
}

.menu-item {
  display: flex;
  align-items: center;
  padding: 10px 16px;
  color: var(--color-text);
  text-decoration: none;
  transition: background-color 0.2s;
  font-size: 14px;
}

.menu-item:hover {
  background-color: var(--color-background-soft);
  color: var(--color-primary);
}

.icon {
  margin-right: 10px;
  width: 16px;
  text-align: center;
}

/* 模态框样式 */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10000;
  backdrop-filter: blur(2px);
}

.modal-content {
  background: var(--color-background);
  width: 90%;
  max-width: 450px;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  animation: fadeIn 0.2s ease-out;
}

.modal-header {
  padding: 16px 20px;
  border-bottom: 1px solid var(--color-border);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-header h3 {
  margin: 0;
  font-size: 18px;
  color: var(--color-heading);
}

.close-btn {
  background: none;
  border: none;
  font-size: 24px;
  color: var(--color-text-soft);
  cursor: pointer;
  line-height: 1;
}

.close-btn:hover {
  color: var(--color-text);
}

.modal-body {
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.avatar-upload-section {
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
}

.avatar-preview {
  width: 80px;
  height: 80px;
  position: relative;
  border-radius: 50%;
  overflow: hidden;
  cursor: pointer;
  border: 2px solid var(--color-border);
}

.avatar-preview img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.avatar-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
  opacity: 0;
  transition: opacity 0.2s;
}

.avatar-preview:hover .avatar-overlay {
  opacity: 1;
}

.avatar-overlay i {
  font-size: 20px;
  margin-bottom: 4px;
}

.avatar-overlay span {
  font-size: 10px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.form-group label {
  font-size: 14px;
  color: var(--color-text);
  font-weight: 500;
}

.form-input {
  padding: 10px;
  border: 1px solid var(--color-border);
  border-radius: 6px;
  background: var(--color-background-soft);
  color: var(--color-text);
  font-size: 14px;
  transition: border-color 0.2s;
}

.form-input:focus {
  outline: none;
  border-color: var(--color-primary);
}

.form-input.disabled {
  background: var(--color-background-mute);
  color: var(--color-text-soft);
  cursor: not-allowed;
}

textarea.form-input {
  resize: vertical;
  min-height: 80px;
}

.modal-footer {
  padding: 16px 20px;
  border-top: 1px solid var(--color-border);
  display: flex;
  justify-content: flex-end;
  gap: 12px;
}

.btn-cancel {
  padding: 8px 16px;
  border: 1px solid var(--color-border);
  background: transparent;
  color: var(--color-text);
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.2s;
}

.btn-cancel:hover {
  background: var(--color-background-soft);
}

.btn-save {
  padding: 8px 20px;
  background: var(--color-primary);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: opacity 0.2s;
}

.btn-save:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.btn-save:hover:not(:disabled) {
  opacity: 0.9;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
