<template>
  <section class="projects-section">
    <div class="section-header">
      <div class="section-number">03</div>
      <h3>项目展示</h3>
    </div>

    <div class="projects-grid">
      <div class="project-card" v-for="(project, index) in projects" :key="index">
        <div class="project-image" :style="{ backgroundColor: project.color }">
          <div class="project-icon">{{ project.icon }}</div>
        </div>
        <div class="project-content">
          <h4>{{ project.title }}</h4>
          <p>{{ project.description }}</p>
          <div class="project-tech">
            <span v-for="(tech, techIndex) in project.technologies" :key="techIndex">
              {{ tech }}
            </span>
          </div>
          <div class="project-links">
            <a href="#" class="project-link">查看项目</a>
            <a href="#" class="project-link">源代码</a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue';

const projects = ref([
  {
    title: "个人博客系统",
    description: "一个使用Vue和Node.js构建的个人博客系统，支持文章发布、评论和用户管理功能。",
    technologies: ["Vue", "Node.js", "MongoDB"],
    icon: "📝",
    color: "rgba(66, 153, 225, 0.2)"
  },
  {
    title: "在线学习平台",
    description: "一个交互式在线学习平台，提供课程管理、视频播放和进度跟踪功能。",
    technologies: ["Vue", "Express", "MySQL"],
    icon: "🎓",
    color: "rgba(72, 187, 120, 0.2)"
  },
  {
    title: "天气预报应用",
    description: "一个实时天气预报应用，使用第三方API获取天气数据，并提供5天预报。",
    technologies: ["JavaScript", "API集成", "CSS动画"],
    icon: "🌤️",
    color: "rgba(237, 137, 54, 0.2)"
  },
  {
    title: "任务管理工具",
    description: "一个简洁的任务管理工具，支持任务创建、分类、优先级设置和截止日期提醒。",
    technologies: ["Vue", "LocalStorage", "Drag & Drop API"],
    icon: "✅",
    color: "rgba(159, 122, 234, 0.2)"
  }
]);
</script>

<style scoped>
.projects-section {
  margin-top: 4rem;
}

.section-header {
  display: flex;
  align-items: center;
  margin-bottom: 2rem;
}

.section-number {
  font-size: 2rem;
  color: #ff6b35;
  margin-right: 1rem;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2rem;
}

.project-card {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.project-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.project-image {
  height: 120px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.project-icon {
  font-size: 3rem;
}

.project-content {
  padding: 1.5rem;
}

.project-content h4 {
  color: #ff6b35;
  margin-bottom: 0.8rem;
  font-size: 1.2rem;
}

.project-content p {
  font-size: 0.9rem;
  color: #2a2a2b;
  margin-bottom: 1rem;
}

.project-tech {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.project-tech span {
  background: rgba(255, 107, 53, 0.2);
  color: #ff6b35;
  padding: 0.3rem 0.6rem;
  border-radius: 20px;
  font-size: 0.8rem;
}

.project-links {
  display: flex;
  gap: 1rem;
}

.project-link {
  color: #7f8fa4;
  font-size: 0.9rem;
  text-decoration: none;
  transition: color 0.3s ease;
}

.project-link:hover {
  color: #ff6b35;
}
</style> 