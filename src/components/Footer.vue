<template>
  <footer class="footer">
    <div class="footer-content">
      <div class="footer-logo">
        <h2>leisure</h2>
        <p>后端开发 | 项目部署 | 数据库设计</p>
      </div>

      <div class="footer-links">
        <div class="footer-links-column">
          <h3>导航</h3>
          <a href="#" @click.prevent="scrollTo('home')">首页</a>
          <a href="#" @click.prevent="scrollTo('skills')">技能</a>
          <a href="#" @click.prevent="scrollTo('projects')">项目</a>
          <a href="#" @click.prevent="scrollTo('contact')">联系</a>
        </div>

        <div class="footer-links-column">
          <h3>社交媒体</h3>
          <a href="#" target="_blank">GitHub</a>
          <a href="#" target="_blank">LinkedIn</a>
          <a href="#" target="_blank">微博</a>
          <a href="#" target="_blank">知乎</a>
        </div>

        <div class="footer-links-column">
          <h3>资源</h3>
          <a href="#" target="_blank">博客</a>
          <a href="#" target="_blank">教程</a>
          <a href="#" target="_blank">资源下载</a>
        </div>
      </div>
    </div>

    <div class="footer-bottom">
      <p>&copy; {{ currentYear }} leisure. 保留所有权利。</p>
      <p>使用 Vue.js 构建 | 由 ❤️ 驱动</p>
    </div>
  </footer>
</template>

<script setup lang="ts">
import { computed } from 'vue'
defineOptions({ name: 'SiteFooter' })

const currentYear = computed(() => new Date().getFullYear())

const scrollTo = (sectionId: string) => {
  const element = document.getElementById(sectionId)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
}
</script>

<style scoped>
.footer {
  background: var(--color-background-soft);
  padding: var(--space-2xl) 0 var(--space-lg);
  margin-top: var(--space-2xl);
  border-top: 1px solid var(--color-border);
  box-shadow: var(--shadow-lg-up);
}

.footer-content {
  max-width: var(--max-width-default);
  margin: 0 auto;
  padding: 0 var(--space-lg);
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: var(--space-xl);
}

.footer-logo h2 {
  color: var(--primary-500);
  font-size: var(--text-2xl);
  margin-bottom: var(--space-xs);
  font-weight: var(--font-bold);
  transition: color var(--transition-normal) var(--ease-out);
}

.footer-logo h2:hover {
  color: var(--primary-600);
}

.footer-logo p {
  color: var(--neutral-500);
  font-size: var(--text-sm);
  line-height: 1.6;
}

.footer-links {
  display: flex;
  gap: var(--space-2xl);
  flex-wrap: wrap;
}

.footer-links-column {
  display: flex;
  flex-direction: column;
  min-width: 160px;
}

.footer-links-column h3 {
  color: var(--color-heading);
  font-size: var(--text-base);
  margin-bottom: var(--space-md);
  font-weight: var(--font-semibold);
}

.footer-links-column a {
  color: var(--neutral-600);
  margin-bottom: var(--space-sm);
  text-decoration: none;
  transition: all var(--transition-normal) var(--ease-out);
  font-size: var(--text-sm);
  position: relative;
  padding: var(--space-xs) 0;
}

.footer-links-column a::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background-color: var(--primary-500);
  transition: width var(--transition-normal) var(--ease-out);
}

.footer-links-column a:hover {
  color: var(--primary-500);
  transform: translateX(var(--space-xs));
}

.footer-links-column a:hover::after {
  width: 100%;
}

.footer-bottom {
  max-width: var(--max-width-default);
  margin: 0 auto;
  padding: var(--space-xl) var(--space-lg) 0;
  border-top: 1px solid var(--color-border);
  margin-top: var(--space-xl);
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: var(--space-md);
}

.footer-bottom p {
  color: var(--neutral-400);
  font-size: var(--text-xs);
}

@media (max-width: 768px) {
  .footer {
    padding: var(--space-xl) 0 var(--space-md);
  }

  .footer-content {
    flex-direction: column;
    padding: 0 var(--space-md);
  }

  .footer-links {
    gap: var(--space-xl);
  }

  .footer-bottom {
    flex-direction: column;
    text-align: center;
    padding: var(--space-lg) var(--space-md) 0;
  }
}
</style>
