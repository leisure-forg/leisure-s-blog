<template>
  <div class="user-avatar" @click="toggleDropdown">
    <img :src="userAvatar" alt="用户头像" class="avatar-img" />
    <div class="dropdown-menu" v-show="isDropdownVisible">
      <div class="user-info">
        <span class="username">{{ username }}</span>
        <span class="email">{{ email }}</span>
      </div>
      <div class="menu-items">
        <router-link to="/profile" class="menu-item">
          <span class="icon">👤</span>
          个人资料
        </router-link>
        <a href="#" class="menu-item" @click.prevent="handleLogout">
          <span class="icon">🚪</span>
          退出登录
        </a>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const isDropdownVisible = ref(false)
const username = ref('admin')
const email = ref('leisure_667010@163.com')
const userAvatar = ref('https://api.dicebear.com/7.x/avataaars/svg?seed=admin')

const toggleDropdown = (): void => {
  isDropdownVisible.value = !isDropdownVisible.value
}

const handleLogout = (): void => {
  // 清除登录状态
  localStorage.removeItem('isLoggedIn')
  // 跳转到登录页
  router.push('/login')
}

// 点击其他地方关闭下拉菜单
const handleClickOutside = (event: MouseEvent): void => {
  const target = event.target as HTMLElement
  if (!target.closest('.user-avatar')) {
    isDropdownVisible.value = false
  }
}

// 添加全局点击事件监听
window.addEventListener('click', handleClickOutside)
</script>

<style scoped>
.user-avatar {
  position: relative;
  cursor: pointer;
  z-index: 9999;
}

.avatar-img {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 2px solid #ff6b35;
  background: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  right: 0;
  width: 200px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
  margin-top: 8px;
  z-index: 9999;
}

.user-info {
  padding: 12px;
  border-bottom: 1px solid #eee;
}

.username {
  display: block;
  font-weight: bold;
  color: #333;
}

.email {
  display: block;
  font-size: 0.8em;
  color: #666;
  margin-top: 4px;
}

.menu-items {
  padding: 8px 0;
}

.menu-item {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  color: #333;
  text-decoration: none;
  transition: background-color 0.2s;
}

.menu-item:hover {
  background-color: #f5f5f5;
}

.icon {
  margin-right: 8px;
}
</style>
