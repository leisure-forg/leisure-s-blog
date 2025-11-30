<template>
  <header class="top-navbar">
    <nav class="nav-links" :class="{ open: menuOpen }">
      <RouterLink to="/" class="link" @click="closeMenu">首页</RouterLink>
      <RouterLink to="/articles" class="link" @click="closeMenu">文章</RouterLink>
      <RouterLink to="/notes" class="link" @click="closeMenu">随记</RouterLink>
      <RouterLink to="/albums" class="link" @click="closeMenu">相册</RouterLink>
      <RouterLink to="/messages" class="link" @click="closeMenu">留言</RouterLink>
      <RouterLink to="/about" class="link" @click="closeMenu">关于我</RouterLink>
    </nav>
    <button class="menu-btn" @click="toggleMenu" aria-label="菜单">
      <span class="bar"></span>
      <span class="bar"></span>
      <span class="bar"></span>
    </button>
    <ThemeToggle class="navbar-theme-toggle" />
  </header>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink, useRouter } from 'vue-router'
import ThemeToggle from './ThemeToggle.vue'

defineOptions({ name: 'TopNavBar' })

const menuOpen = ref(false)
const toggleMenu = () => (menuOpen.value = !menuOpen.value)
const closeMenu = () => (menuOpen.value = false)

const router = useRouter()
const goHome = () => router.push('/')
</script>

<style scoped>
.top-navbar {
  position: sticky;
  top: 0;
  left: 0;
  right: 0;
  z-index: 10000;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-sm) var(--space-lg);
  background: var(--color-background);
  border-bottom: 1px solid var(--color-border);
  box-shadow: var(--shadow-sm);
}

:deep(.navbar-theme-toggle) {
  margin: 0;
}

.nav-links {
  display: none;
  gap: var(--space-md);
}

.link {
  color: var(--color-text);
  text-decoration: none;
  font-size: var(--text-sm);
}

.menu-btn {
  width: 36px;
  height: 28px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  background: transparent;
  border: none;
  cursor: pointer;
}

.bar {
  width: 100%;
  height: 3px;
  background: var(--color-text);
  border-radius: var(--radius-full);
}

@media (min-width: 550px) {
  .nav-links {
    display: flex;
  }
  .menu-btn {
    display: none;
  }
}

/* 移动端抽屉式导航 */
@media (max-width: 549px) {
  .nav-links {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    display: flex;
    flex-direction: column;
    background: var(--color-background);
    border-bottom: 1px solid var(--color-border);
    padding: var(--space-sm) var(--space-lg);
    box-shadow: var(--shadow-md);
    max-height: 0;
    overflow: hidden;
    transition: max-height var(--transition-normal) var(--ease-out);
  }
  .nav-links.open {
    max-height: 300px;
  }
  .link {
    padding: var(--space-xs) 0;
  }
}
</style>
