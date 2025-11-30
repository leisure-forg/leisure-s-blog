<template>
  <main class="main-content">
    <section id="home" class="hero">
      <h2>你好，我是 <span>leisure</span></h2>
      <p>江西理工大学在校大二学生，方向：后端开发 / AI应用开发工程师</p>

      <div class="stats">
        <div class="stat-card">
          <div class="number">2</div>
          <div class="label">年编程经验</div>
        </div>
        <div class="stat-card">
          <div class="number">15+</div>
          <div class="label">完成项目</div>
        </div>
        <div class="stat-card">
          <div class="number">10+</div>
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
        我是江西理工大学在校大二学生，方向为后端开发与AI应用开发。拥有2年编程经验，已完成15+项目，技术栈覆盖Java/Go/Node.js后端、数据库与消息队列、LangChain与RAG等AI技术，以及Docker与Nginx的运维部署与Vue/React前端开发。
      </p>
      <p>我热衷将创意转化为工程化的网页与应用系统，并持续探索分布式架构与智能代理的融合实践。</p>

      <div class="timeline">
        <div class="timeline-item">
          <div class="timeline-dot"></div>
          <div class="timeline-date">2024-12</div>
          <div class="timeline-content">
            <h4>编程启蒙阶段</h4>
            <p>掌握Java核心与WebSocket，独立开发局域网聊天应用</p>
          </div>
        </div>

        <div class="timeline-item">
          <div class="timeline-dot"></div>
          <div class="timeline-date">2025-1</div>
          <div class="timeline-content">
            <h4>Java进阶开发</h4>
            <p>深入多线程与网络编程，开发Java版坦克大战</p>
          </div>
        </div>

        <div class="timeline-item">
          <div class="timeline-dot"></div>
          <div class="timeline-date">2025-5</div>
          <div class="timeline-content">
            <h4>企业级应用开发</h4>
            <p>掌握SSM框架与数据库技术，设计并实现校园外卖平台</p>
          </div>
        </div>

        <div class="timeline-item">
          <div class="timeline-dot"></div>
          <div class="timeline-date">2025-7</div>
          <div class="timeline-content">
            <h4>AI技术突破</h4>
            <p>深入学习LangChain与向量数据库，构建并优化RAG系统</p>
          </div>
        </div>
        <div class="timeline-item">
          <div class="timeline-dot"></div>
          <div class="timeline-date">2025-8-9</div>
          <div class="timeline-content">
            <h4>全栈能力拓展</h4>
            <p>扩展Go与Node.js后端技术栈，参与多项全栈项目实战</p>
          </div>
        </div>
        <div class="timeline-item">
          <div class="timeline-dot"></div>
          <div class="timeline-date">2025-10-11</div>
          <div class="timeline-content">
            <h4>技术深度整合</h4>
            <p>研究消息队列与分布式系统，探索模型微调与Agent系统开发</p>
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

<script setup lang="ts">
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
