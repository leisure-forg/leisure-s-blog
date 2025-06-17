<template>
  <div class="sidebar-extra">
    <!-- 心情/格言区 -->
    <section class="mood-section">
      <div class="mood-title">🌞 原来长大并不简单</div>
      <div class="mood-desc">一点都不想长大,长大后所有的事情都会变得复杂。</div>
      <div class="mood-time">2025-04-21</div>
      <div class="mood-quote">时光是怎么平静划刻录，苦尝蜕刻的鸿蒙里，终将开出名为坚韧的花。</div>
      <div class="mood-time">2025-05-29</div>
    </section>
    <!-- 文章分类区 -->
    <section class="category-section">
      <div class="section-title">文章分类 <span class="count">共5个分类</span></div>
      <ul class="category-list">
        <li>其它 <span class="cat-count">1篇</span></li>
        <li>工具推荐 <span class="cat-count">0篇</span></li>
        <li>前端开发 <span class="cat-count">0篇</span></li>
      </ul>
    </section>
    <!-- 标签区 -->
    <section class="tag-section">
      <div class="section-title">文章标签 <span class="count">共10个标签</span></div>
      <div class="tag-list">
        <span class="tag">JavaScript</span>
        <span class="tag">Vue</span>
        <span class="tag">WebSocket</span>
        <span class="tag">redis</span>
        <span class="tag">SpringBoot</span>
        <span class="tag">前端</span>
        <span class="tag">随记</span>
        <span class="tag">工具</span>
        <span class="tag">生活</span>
        <span class="tag">技术</span>
      </div>
    </section>
  </div>
</template>

<script setup>
import ThemeToggle from './ThemeToggle.vue'
</script>

<style scoped>
.sidebar-extra {
  background: var(--theme-background);
  border-radius: 16px;
  box-shadow: 0 2px 8px var(--theme-shadow);
  padding: 24px 20px;
  font-size: 1rem;
  height: 100%;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.theme-section {
  margin-bottom: 32px;
  padding-bottom: 16px;
  border-bottom: 1px solid var(--theme-background-soft);
  display: flex;
  justify-content: center;
}

.mood-section {
  margin-bottom: 32px;
  padding-bottom: 16px;
  border-bottom: 1px solid var(--theme-background-soft);
}
.mood-title {
  font-weight: bold;
  font-size: 1.1rem;
  margin-bottom: 8px;
  color: var(--theme-primary-dark);
}
.mood-desc {
  color: var(--neutral-600);
  margin-bottom: 4px;
}
.mood-time {
  color: var(--neutral-400);
  font-size: 0.92em;
  margin-bottom: 6px;
}
.mood-quote {
  color: var(--theme-primary);
  font-size: 0.98em;
  margin-bottom: 4px;
}
.section-title {
  font-weight: bold;
  margin-bottom: 10px;
  font-size: 1.05em;
  color: var(--theme-primary-dark);
}
.count {
  color: var(--neutral-400);
  font-size: 0.95em;
  margin-left: 8px;
}
.category-list {
  list-style: none;
  padding: 0;
  margin: 0;
}
.category-list li {
  display: flex;
  justify-content: space-between;
  padding: 6px 0;
  color: var(--neutral-700);
}
.cat-count {
  color: var(--theme-primary);
  font-size: 0.95em;
}
.tag-section {
  margin-top: 32px;
}
.tag-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px 12px;
  margin-top: 8px;
}
.tag {
  background: var(--theme-tag-bg);
  color: var(--theme-primary-dark);
  border-radius: 12px;
  padding: 4px 14px;
  font-size: 0.98em;
  transition:
    background 0.2s,
    color 0.2s,
    transform 0.2s;
  cursor: pointer;
}

.tag:hover {
  transform: translateY(-1px);
}
.tag:hover {
  background: var(--theme-primary);
  color: #fff;
}
</style>
