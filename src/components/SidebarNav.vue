<template>
  <nav class="sidebar-nav">
    <ul>
      <li>
        <router-link to="/"><span class="icon">🏠</span> 首页</router-link>
      </li>
      <li>
        <a href="#"><span class="icon">📄</span> 文章</a>
      </li>
      <li>
        <a href="#"><span class="icon">📝</span> 随记</a>
      </li>
      <li>
        <a href="#"><span class="icon">💬</span> 留言</a>
      </li>
      <li>
        <a href="#"><span class="icon">🖼️</span> 相册</a>
      </li>
      <li>
        <router-link to="/about"><span class="icon">👤</span> 关于我</router-link>
      </li>
      <li>
        <a href="#"><span class="icon">🔗</span> 友链</a>
      </li>
    </ul>
  </nav>
</template>

<script setup>
// 暂无逻辑
</script>

<script>
export default {}
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
}
.icon {
  margin-right: 12px;
  font-size: 1.2em;
}
</style>
