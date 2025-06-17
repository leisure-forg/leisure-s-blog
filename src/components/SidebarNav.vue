<template>
  <nav class="sidebar-nav">
    <router-link to="/" class="nav-item">
      <IconHome class="icon" />
      <span>首页</span>
    </router-link>
    <router-link to="/articles" class="nav-item">
      <IconDocumentation class="icon" />
      <span>文章</span>
    </router-link>
    <router-link to="/notes" class="nav-item">
      <!-- <i class="fas fa-sticky-note"></i> -->
      <IconCommunity class="icon" />
      <span>随记</span>
    </router-link>
    <router-link to="/albums" class="nav-item">
      <i class="fas fa-images"></i>
      <span>相册</span>
    </router-link>
    <router-link to="/messages" class="nav-item">
      <i class="fas fa-comments"></i>
      <span>留言</span>
    </router-link>
    <router-link to="/about" class="nav-item">
      <i class="fas fa-user"></i>
      <span>关于我</span>
    </router-link>
  </nav>
</template>

<script setup>
import IconCommunity from './icons/IconCommunity.vue'
import IconDocumentation from './icons/IconDocumentation.vue'
//import IconEcosystem from './icons/IconEcosystem.vue'
import IconHome from './icons/IconHome.vue'
//import IconSupport from './icons/IconSupport.vue'
//import IconTooling from './icons/IconTooling.vue'
</script>

<style scoped>
.sidebar-nav {
  background: var(--color-background-soft);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-md);
  padding: var(--space-lg) 0;
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  transition: box-shadow var(--transition-normal) var(--ease-out);
}

.sidebar-nav:hover {
  box-shadow: var(--shadow-lg);
}

.nav-item {
  display: flex;
  align-items: center;
  padding: var(--space-sm) var(--space-xl);
  color: var(--color-text);
  text-decoration: none;
  font-size: var(--text-base);
  font-weight: var(--font-medium);
  border-left: 4px solid transparent;
  margin: var(--space-xs) 0;
  transition: all var(--transition-normal) var(--ease-out);
  position: relative;
  overflow: hidden;
}

.nav-item::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: var(--primary-100);
  transform: translateX(-100%);
  transition: transform var(--transition-normal) var(--ease-out);
  z-index: -1;
}

.nav-item:hover {
  color: var(--primary-600);
  border-left: 4px solid var(--primary-500);
  transform: translateX(var(--space-xs));
}

.nav-item:hover::before {
  transform: translateX(0);
}

.nav-item.router-link-active {
  color: var(--primary-600);
  border-left: 4px solid var(--primary-500);
  background-color: var(--primary-50);
  font-weight: var(--font-semibold);
}

.icon,
.fas {
  margin-right: var(--space-md);
  font-size: var(--text-lg);
  transition: transform var(--transition-normal) var(--ease-out);
  color: var(--neutral-500);
}

.nav-item:hover .icon,
.nav-item:hover .fas {
  transform: scale(1.2);
  color: var(--primary-500);
}

.nav-item.router-link-active .icon,
.nav-item.router-link-active .fas {
  color: var(--primary-500);
}

/* 添加响应式设计 */
@media (max-width: 768px) {
  .sidebar-nav {
    padding: var(--space-md) 0;
  }

  .nav-item {
    padding: var(--space-xs) var(--space-md);
  }

  .icon,
  .fas {
    margin-right: var(--space-sm);
  }
}
</style>
