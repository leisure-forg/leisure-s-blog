<script setup lang="ts">
import TopVideoHeader from './components/TopVideoHeader.vue'
import TopNavBar from './components/TopNavBar.vue'
import SidebarProfile from './components/SidebarProfile.vue'
import SidebarNav from './components/SidebarNav.vue'
import SidebarExtra from './components/SidebarExtra.vue'
import UserAvatar from './components/UserAvatar.vue'
import ThemeToggle from './components/ThemeToggle.vue'
import Live2DWidget from './components/Live2DWidget.vue'
import { RouterView, useRoute } from 'vue-router'
import { computed, ref, onMounted, onBeforeUnmount, reactive } from 'vue'
import './assets/base.css'

const route = useRoute()
const isLoggedIn = computed(() => localStorage.getItem('isLoggedIn') === 'true')
const isAuthPage = computed(() => ['login', 'register'].includes(route.name as string))
const shouldShowSidebarExtra = computed(() => {
  return (
    !isAuthPage.value &&
    !route.path.includes('detail') &&
    !String(route.name).toLowerCase().includes('detail')
  )
})

const isMobile = ref(false)
const updateIsMobile = () => {
  isMobile.value = window.matchMedia('(max-width: 900px)').matches
}

// 头像拖拽逻辑
const avatarPosition = reactive({ x: 0, y: 0 })
const isDragging = ref(false)
const dragOffset = { x: 0, y: 0 }

const startDrag = (e: MouseEvent) => {
  // 如果点击的是头像内部的交互元素（如菜单项），则不触发拖拽
  // 这里简单处理，只要按下就触发，利用 preventDefault 防止选择文本
  e.preventDefault()
  isDragging.value = true
  dragOffset.x = e.clientX - avatarPosition.x
  dragOffset.y = e.clientY - avatarPosition.y

  document.addEventListener('mousemove', onDrag)
  document.addEventListener('mouseup', stopDrag)
}

const onDrag = (e: MouseEvent) => {
  if (!isDragging.value) return
  avatarPosition.x = e.clientX - dragOffset.x
  avatarPosition.y = e.clientY - dragOffset.y
}

const stopDrag = () => {
  isDragging.value = false
  document.removeEventListener('mousemove', onDrag)
  document.removeEventListener('mouseup', stopDrag)
}

const startTouchDrag = (e: TouchEvent) => {
  isDragging.value = true
  const touch = e.touches[0]
  dragOffset.x = touch.clientX - avatarPosition.x
  dragOffset.y = touch.clientY - avatarPosition.y

  document.addEventListener('touchmove', onTouchDrag, { passive: false })
  document.addEventListener('touchend', stopTouchDrag)
}

const onTouchDrag = (e: TouchEvent) => {
  if (!isDragging.value) return
  if (e.cancelable) e.preventDefault()

  const touch = e.touches[0]
  avatarPosition.x = touch.clientX - dragOffset.x
  avatarPosition.y = touch.clientY - dragOffset.y
}

const stopTouchDrag = () => {
  isDragging.value = false
  document.removeEventListener('touchmove', onTouchDrag)
  document.removeEventListener('touchend', stopTouchDrag)
}

onMounted(() => {
  updateIsMobile()
  window.addEventListener('resize', updateIsMobile)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', updateIsMobile)
})
</script>

<template>
  <div class="app">
    <Live2DWidget />
    <div v-if="isAuthPage">
      <RouterView />
    </div>
    <div v-else class="main-layout">
      <TopNavBar v-if="isMobile" />
      <TopVideoHeader v-else />
      <div class="content-row">
        <aside v-if="!isMobile" class="sidebar-left">
          <SidebarProfile />
          <SidebarNav />
          <div class="theme-toggle-wrapper">
            <ThemeToggle />
          </div>
        </aside>
        <main class="main-content">
          <RouterView />
        </main>
        <aside v-if="shouldShowSidebarExtra && !isMobile" class="sidebar-right">
          <SidebarExtra />
        </aside>
      </div>
      <div
        class="user-avatar-container"
        v-if="isLoggedIn"
        @mousedown="startDrag"
        @touchstart="startTouchDrag"
        :style="{
          transform: `translate(${avatarPosition.x}px, ${avatarPosition.y}px)`,
          cursor: isDragging ? 'grabbing' : 'grab',
        }"
      >
        <UserAvatar />
      </div>
    </div>
  </div>
</template>

<style>
html,
body,
#app {
  width: 100%;
  height: 100vh;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>

<style scoped>
.app {
  width: 100%;
  min-height: 100vh;
}

.main-layout {
  width: 100%;
  min-height: 100vh;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  padding-top: 0;
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
}

.content-row {
  display: flex;
  width: 100%;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  position: relative;
  z-index: 2;
  overflow: visible;
  flex: 1;
  height: calc(100vh - 56px);
}

.sidebar-left {
  width: 280px;
  margin: 0;
  align-self: stretch;
  border-radius: 0 16px 16px 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  height: 100%;
}

.main-content {
  flex: 1 1 0;
  min-width: 0;
  margin: 0 16px;
  position: relative;
  z-index: 2;
}

.sidebar-right {
  width: 300px;
  margin: 0;
  align-self: stretch;
  border-radius: 16px 0 0 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  flex-shrink: 0;
}

.theme-toggle-wrapper {
  padding: var(--space-md);
  margin-top: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  border-top: 1px solid var(--color-border);
}

.user-avatar-container {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 99999;
  touch-action: none;
}

@media (max-width: 1200px) {
  .sidebar-left,
  .sidebar-right {
    width: 200px;
  }
}

@media (max-width: 900px) {
  .content-row {
    flex-direction: column;
    align-items: stretch;
    width: 100%;
  }
  .main-content {
    margin: 0;
  }
}

header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
