<template>
  <section class="projects-section" id="projects">
    <div class="section-header">
      <div class="section-number">03</div>
      <h3>项目展示</h3>
    </div>

    <div class="projects-grid">
      <div
        class="project-card"
        v-for="(project, index) in projects"
        :key="index"
        @mouseenter="project.isHovered = true"
        @mouseleave="project.isHovered = false"
        :class="{ 'is-hovered': project.isHovered }"
      >
        <div
          class="project-image"
          :style="{
            background: `linear-gradient(135deg, ${project.gradientStart}, ${project.gradientEnd})`,
          }"
        >
          <img v-if="project.image" :src="project.image" :alt="project.title" class="project-img" />
        </div>
        <div class="project-content">
          <h4>{{ project.title }}</h4>
          <p>{{ project.description }}</p>
          <div class="project-tech">
            <span
              v-for="(tech, techIndex) in project.technologies"
              :key="techIndex"
              :style="{
                transitionDelay: project.isHovered ? `${techIndex * 0.1}s` : '0s',
                transform: project.isHovered
                  ? 'translateY(0) scale(1)'
                  : 'translateY(5px) scale(0.95)',
                opacity: project.isHovered ? 1 : 0.8,
              }"
            >
              {{ tech }}
            </span>
          </div>
          <div class="project-links">
            <a href="#" class="project-link" @click.prevent="handleViewProject(project)">
              <span>查看项目</span>
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"></path>
                <polyline points="15 3 21 3 21 9"></polyline>
                <line x1="10" y1="14" x2="21" y2="3"></line>
              </svg>
            </a>
            <a href="#" class="project-link" @click.prevent="handleViewSource(project)">
              <span>源代码</span>
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path
                  d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"
                ></path>
              </svg>
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import ragImg from '../static/picture/rag.jpg'
import takeoutImg from '../static/picture/takeout.png'
import networkImg from '../static/picture/network.png'
import aiImg from '../static/picture/ai.png'
defineOptions({ name: 'ProjectsSection' })

type ProjectItem = {
  title: string
  description: string
  technologies: string[]
  image?: string
  gradientStart: string
  gradientEnd: string
  isHovered: boolean
  isVisible: boolean
}

const projects = ref<ProjectItem[]>([
  {
    title: '智能文档检索系统（公司项目:闭源）',
    description:
      '基于LangChain与向量数据库构建端到端RAG系统，支持多格式文档智能问答，并通过动态文本切割与重排序优化召回准确率。',
    technologies: ['LangChain', '向量数据库', 'RAG', '重排序'],
    image: ragImg,
    gradientStart: 'rgba(66, 153, 225, 0.6)',
    gradientEnd: 'rgba(66, 153, 225, 0.2)',
    isHovered: false,
    isVisible: false,
  },
  {
    title: '校园外卖平台（个人项目:闭源）',
    description:
      '采用Spring Boot与Gin构建微服务架构，实施多级缓存策略并结合Nginx负载均衡与Docker容器化，实现高并发下的稳定运行。',
    technologies: ['Spring Boot', 'Gin', 'Redis', 'Docker', 'Nginx'],
    image: takeoutImg,
    gradientStart: 'rgba(72, 187, 120, 0.6)',
    gradientEnd: 'rgba(72, 187, 120, 0.2)',
    isHovered: false,
    isVisible: false,
  },
  {
    title: '多Agent任务协作平台（公司项目:闭源）',
    description:
      '基于ReAct框架构建智能任务规划与多工具协作能力，集成MCP协议扩展，实现多Agent在复杂任务中的协同处理。',
    technologies: ['ReAct', 'Agent', '工具调用', 'MCP'],
    image: aiImg,
    gradientStart: 'rgba(237, 137, 54, 0.6)',
    gradientEnd: 'rgba(237, 137, 54, 0.2)',
    isHovered: false,
    isVisible: false,
  },
  {
    title: '江理校园网自动连接工具（个人项目:开源）',
    description:
      '使用PyQt开发桌面端界面，Go后端常驻服务检测网络状态并自动触发认证，提供稳定的校园网自动登录体验。',
    technologies: ['PyQt', 'Go', 'Windows服务', '网络认证'],
    image: networkImg,
    gradientStart: 'rgba(159, 122, 234, 0.6)',
    gradientEnd: 'rgba(159, 122, 234, 0.2)',
    isHovered: false,
    isVisible: false,
  },
])

const isCampusProject = (p: ProjectItem) => p.title.includes('江理校园网自动连接工具')
const handleViewProject = (p: ProjectItem) => {
  if (isCampusProject(p)) {
    window.open(
      'https://leisure01-1745393177479-9855.oss-cn-beijing.aliyuncs.com/CampusNet-AutoLogin/login.zip',
      '_blank',
    )
  } else {
    alert('暂不提供该项目的在线访问')
  }
}

const handleViewSource = (p: ProjectItem) => {
  if (isCampusProject(p)) {
    window.open('https://github.com/leisure-forg/CampusNet-AutoLogin', '_blank')
  } else {
    alert('该项目源代码暂不公开')
  }
}

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          // 为项目卡片添加进入视图时的动画
          projects.value.forEach((project, index) => {
            setTimeout(() => {
              project.isVisible = true
            }, index * 200)
          })
        }
      })
    },
    {
      threshold: 0.1,
    },
  )

  // 观察项目部分
  const projectsSection = document.getElementById('projects')
  if (projectsSection) {
    observer.observe(projectsSection)
  }
})
</script>

<style scoped>
.projects-section {
  margin-top: var(--space-3xl);
  position: relative;
}

.section-header {
  display: flex;
  align-items: center;
  margin-bottom: var(--space-xl);
  position: relative;
}

.section-header::after {
  content: '';
  position: absolute;
  bottom: -10px;
  left: 0;
  width: 60px;
  height: 3px;
  background: var(--primary-500);
  border-radius: var(--radius-full);
}

.section-number {
  font-size: var(--text-3xl);
  color: var(--primary-500);
  margin-right: var(--space-md);
  font-weight: var(--font-bold);
  opacity: 0.8;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: var(--space-xl);
  perspective: 1000px;
}

.project-card {
  background: var(--color-background-soft);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-md);
  overflow: hidden;
  transition: all var(--transition-normal) var(--ease-out);
  border: 1px solid var(--color-border);
  transform-style: preserve-3d;
  position: relative;
}

.project-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    45deg,
    transparent 0%,
    rgba(255, 255, 255, 0.1) 50%,
    transparent 100%
  );
  transform: translateY(100%);
  transition: transform 0.6s var(--ease-out);
}

.project-card:hover {
  transform: translateY(-8px) rotateX(4deg);
  box-shadow: var(--shadow-lg);
  border-color: var(--primary-200);
}

.project-card:hover::before {
  transform: translateY(-100%);
}

.project-image {
  height: 140px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

.project-image::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 40%;
  background: linear-gradient(to top, var(--color-background-soft) 0%, transparent 100%);
}

.project-icon {
  font-size: var(--text-4xl);
  transform: scale(1);
  transition: transform 0.3s var(--ease-out);
}

.project-card:hover .project-icon {
  transform: scale(1.2) rotate(8deg);
}

.project-img {
  width: 72px;
  height: 72px;
  object-fit: cover;
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-sm);
  transform: scale(1);
  transition: transform 0.3s var(--ease-out);
}

.project-card:hover .project-img {
  transform: scale(1.1);
}

.project-content {
  padding: var(--space-lg);
  position: relative;
}

.project-content h4 {
  color: var(--primary-600);
  margin-bottom: var(--space-sm);
  font-size: var(--text-lg);
  font-weight: var(--font-bold);
  position: relative;
  display: inline-block;
}

.project-content h4::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--primary-500);
  transition: width 0.3s var(--ease-out);
}

.project-card:hover h4::after {
  width: 100%;
}

.project-content p {
  font-size: var(--text-sm);
  color: var(--color-text);
  margin-bottom: var(--space-md);
  line-height: 1.6;
}

.project-tech {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-xs);
  margin-bottom: var(--space-lg);
}

.project-tech span {
  background: var(--primary-50);
  color: var(--primary-600);
  padding: var(--space-xs) var(--space-sm);
  border-radius: var(--radius-full);
  font-size: var(--text-xs);
  font-weight: var(--font-medium);
  transition: all var(--transition-normal) var(--ease-out);
}

.project-tech span:hover {
  background: var(--primary-100);
  transform: translateY(-2px);
}

.project-links {
  display: flex;
  gap: var(--space-md);
  opacity: 0;
  transform: translateY(10px);
  transition: all 0.3s var(--ease-out);
}

.project-card:hover .project-links {
  opacity: 1;
  transform: translateY(0);
}

.project-link {
  color: var(--neutral-600);
  font-size: var(--text-sm);
  text-decoration: none;
  transition: all var(--transition-normal) var(--ease-out);
  position: relative;
  padding: var(--space-xs) 0;
}

.project-link::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--primary-500);
  transition: width 0.3s var(--ease-out);
}

.project-link:hover {
  color: var(--primary-600);
}

.project-link:hover::after {
  width: 100%;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .projects-section {
    margin-top: var(--space-xl);
  }

  .projects-grid {
    grid-template-columns: 1fr;
    gap: var(--space-lg);
  }

  .project-card {
    transform: none !important;
  }

  .project-image {
    height: 120px;
  }

  .project-content {
    padding: var(--space-md);
  }

  .project-links {
    opacity: 1;
    transform: none;
  }
}
</style>
const isCampusProject = (p: ProjectItem) => p.title.includes('江理校园网自动连接工具') const
handleViewProject = (p: ProjectItem) => { if (isCampusProject(p)) {
window.open('https://leisure01-1745393177479-9855.oss-cn-beijing.aliyuncs.com/CampusNet-AutoLogin/login.zip',
'_blank') } else { alert('暂不提供该项目的在线访问') } } const handleViewSource = (p: ProjectItem)
=> { if (isCampusProject(p)) { window.open('https://github.com/leisure-forg/CampusNet-AutoLogin',
'_blank') } else { alert('该项目源代码暂不公开') } }
