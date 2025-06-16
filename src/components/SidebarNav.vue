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
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  padding: 20px 0;
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
}
.sidebar-nav ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.sidebar-nav li {
  margin: 0;
}
.sidebar-nav a {
  display: flex;
  align-items: center;
  padding: 12px 32px;
  color: #333;
  text-decoration: none;
  font-size: 1rem;
  border-left: 4px solid transparent;
  transition:
    background 0.2s,
    border-color 0.2s,
    color 0.2s;
}
.sidebar-nav a:hover {
  background: #f0f6ff;
  color: #409eff;
  border-left: 4px solid #409eff;
  scale: 1.05;
  transition: 0.2s;
}
.icon {
  margin-right: 12px;
  font-size: 1.2em;
}
.fas {
  margin-right: 12px;
  font-size: 1.2em;
}
</style>
