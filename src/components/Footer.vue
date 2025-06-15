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

<script setup>
import { computed } from 'vue';

const currentYear = computed(() => new Date().getFullYear());

const scrollTo = (sectionId) => {
  const element = document.getElementById(sectionId);
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' });
  }
};
</script>

<style scoped>
.footer {
  background: rgba(0, 10, 20, 0.9);
  padding: 3rem 0 1.5rem;
  margin-top: 2rem;
  border-top: 1px solid rgba(255, 107, 53, 0.3);
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 2rem;
}

.footer-logo h2 {
  color: #ff6b35;
  font-size: 1.8rem;
  margin-bottom: 0.5rem;
}

.footer-logo p {
  color: #7f8fa4;
  font-size: 0.9rem;
}

.footer-links {
  display: flex;
  gap: 3rem;
  flex-wrap: wrap;
}

.footer-links-column {
  display: flex;
  flex-direction: column;
}

.footer-links-column h3 {
  color: #e6f1ff;
  font-size: 1rem;
  margin-bottom: 1rem;
}

.footer-links-column a {
  color: #7f8fa4;
  margin-bottom: 0.8rem;
  text-decoration: none;
  transition: color 0.3s ease;
  font-size: 0.9rem;
}

.footer-links-column a:hover {
  color: #ff6b35;
}

.footer-bottom {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem 2rem 0;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  margin-top: 2rem;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 1rem;
}

.footer-bottom p {
  color: #7f8fa4;
  font-size: 0.8rem;
}

@media (max-width: 768px) {
  .footer-content {
    flex-direction: column;
  }
  
  .footer-bottom {
    flex-direction: column;
    text-align: center;
  }
}
</style> 