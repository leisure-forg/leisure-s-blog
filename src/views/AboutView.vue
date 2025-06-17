<template>
  <main class="main-content">
    <section id="home" class="hero">
      <h2>你好，我是 <span>leisure</span></h2>
      <p>一名热爱编程的大学生，专注于后端开发</p>

      <div class="stats">
        <div class="stat-card">
          <div class="number">1</div>
          <div class="label">年编程经验</div>
        </div>
        <div class="stat-card">
          <div class="number">3+</div>
          <div class="label">完成项目</div>
        </div>
        <div class="stat-card">
          <div class="number">5</div>
          <div class="label">技术栈</div>
        </div>
      </div>
    </section>

    <section id="story" class="story">
      <div class="section-header">
        <div class="section-number">01</div>
        <h3>我的故事</h3>
      </div>
      <p>
        我是一名江西理工大学的学生，热爱编程和技术创新。在过去的两年里，我专注于学习后端开发与少量前端技术，包括Java,Spring,SpringMvc,Springboot,myBatis,Redis,nginx,maven,docker,linux,git,mysql,JDBC,JWT和html,css,js,vue等。
      </p>
      <p>我喜欢将创意转化为实际的网页和应用程序，并且一直在寻找新的挑战和学习机会。</p>

      <div class="timeline">
        <div class="timeline-item">
          <div class="timeline-dot"></div>
          <div class="timeline-date">2024-12</div>
          <div class="timeline-content">
            <h4>开始编程之旅</h4>
            <p>接触Java,和WebSocket，创建了第一个局域网聊天应用</p>
          </div>
        </div>

        <div class="timeline-item">
          <div class="timeline-dot"></div>
          <div class="timeline-date">2025-1</div>
          <div class="timeline-content">
            <h4>深入Java</h4>
            <p>学习Java基础和进阶知识，利用多线程开始成功开发Java版坦克大战</p>
          </div>
        </div>

        <div class="timeline-item">
          <div class="timeline-dot"></div>
          <div class="timeline-date">2025-5</div>
          <div class="timeline-content">
            <h4>框架学习</h4>
            <p>深入学习SSM架构，和数据库技术,构建了第一个校内外卖平台应用</p>
          </div>
        </div>

        <div class="timeline-item">
          <div class="timeline-dot"></div>
          <div class="timeline-date">2025-5</div>
          <div class="timeline-content">
            <h4>不远的未来</h4>
            <p>学习SpringCloud和SpringSecurity以及SpringAI等技术,将ai集成到我的应用程序当中</p>
          </div>
        </div>
      </div>
    </section>

    <!-- 引入技能组件 -->
    <div id="skills">
      <Skills />
    </div>

    <!-- 引入项目组件 -->
    <div id="projects">
      <Projects />
    </div>

    <!-- 引入联系表单组件 -->
    <div id="contact">
      <ContactForm />
    </div>

    <!-- 引入页脚组件 -->
    <div id="footer">
      <Footer />
    </div>
  </main>
</template>

<script setup>
import Skills from '../components/Skills.vue'
import Projects from '../components/Projects.vue'
import ContactForm from '../components/ContactForm.vue'
import Footer from '../components/Footer.vue'
</script>

<style scoped>
.main-content {
  flex: 1;
  padding: 3rem;
  overflow-y: auto;
}

.hero h2 {
  font-size: 3rem;
  margin-bottom: 2rem;
}

.hero span {
  color: var(--primary-500);
}

.stats {
  display: flex;
  gap: 2rem;
  margin: 3rem 0;
}

.stat-card {
  background: rgba(255, 255, 255, 0.1);
  padding: 1.5rem;
  border-radius: 10px;
  box-shadow: 1px 2px 4px rgba(0, 0, 0, 0.1);
  text-align: center;
  flex: 1;
}

.number {
  font-size: 2.5rem;
  color: #ee6f3d;
  font-weight: bold;
}

.label {
  color: rgb(3, 3, 31);
  margin-top: 0.5rem;
}

.section-header {
  display: flex;
  align-items: center;
  margin-bottom: 2rem;
}

.section-number {
  font-size: 2rem;
  color: var(--primary-500);
  margin-right: 1rem;
}

/* 时间线样式 */
.timeline {
  position: relative;
  margin: 3rem 0;
  padding-left: 2rem;
}

.timeline::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  width: 2px;
  background: var(--primary-500);
}

.timeline-item {
  position: relative;
  margin-bottom: 2rem;
}

.timeline-dot {
  position: absolute;
  left: -2.4rem;
  top: 0.3rem;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: var(--primary-500);
}

.timeline-date {
  color: var(--primary-500);
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.timeline-content h4 {
  margin-bottom: 0.5rem;
}

.timeline-content p {
  color: #7f8fa4;
}

/* 响应式调整 */
@media (max-width: 768px) {
  .main-content {
    padding: 2rem;
  }

  .hero h2 {
    font-size: 2rem;
  }

  .stats {
    flex-direction: column;
  }
}
</style>
