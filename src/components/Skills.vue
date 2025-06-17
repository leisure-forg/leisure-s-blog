<template>
  <section class="skills-section" id="skills">
    <div class="section-header">
      <div class="section-number">02</div>
      <h3>技能专长</h3>
    </div>

    <div class="skills-container">
      <div class="skill-category" v-for="(category, index) in skillCategories" :key="index">
        <h4>{{ category.name }}</h4>
        <div class="skills-grid">
          <div
            class="skill-item"
            v-for="(skill, skillIndex) in category.skills"
            :key="skillIndex"
            @mouseenter="skill.isHovered = true"
            @mouseleave="skill.isHovered = false"
          >
            <div class="skill-name" :data-level="`${skill.level}%`">
              {{ skill.name }}
            </div>
            <div class="skill-bar">
              <div
                class="skill-level"
                :style="{ width: skill.isVisible ? `${skill.level}%` : '0%' }"
                ref="skillLevels"
              ></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const skillCategories = ref([
  {
    name: '后端开发',
    skills: [
      { name: 'Java', level: 90, isHovered: false, isVisible: false },
      { name: 'MySQL', level: 85, isHovered: false, isVisible: false },
      { name: 'Vue.js', level: 80, isHovered: false, isVisible: false },
    ],
  },
  {
    name: '工具和框架',
    skills: [
      { name: 'Spring Boot', level: 70, isHovered: false, isVisible: false },
      { name: 'Git', level: 65, isHovered: false, isVisible: false },
      { name: 'Docker', level: 60, isHovered: false, isVisible: false },
    ],
  },
])

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          // 找到对应的技能并设置其为可见
          skillCategories.value.forEach((category) => {
            category.skills.forEach((skill) => {
              if (!skill.isVisible) {
                setTimeout(() => {
                  skill.isVisible = true
                }, 200)
              }
            })
          })
        }
      })
    },
    {
      threshold: 0.1,
    },
  )

  // 观察技能部分
  const skillsSection = document.getElementById('skills')
  if (skillsSection) {
    observer.observe(skillsSection)
  }
})
</script>

<style scoped>
.skills-section {
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

.skills-container {
  display: flex;
  flex-direction: column;
  gap: var(--space-xl);
}

.skill-category {
  background: var(--color-background-soft);
  padding: var(--space-xl);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-md);
  transition: all var(--transition-normal) var(--ease-out);
  border: 1px solid var(--color-border);
  position: relative;
  overflow: hidden;
}

.skill-category:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-lg);
  border-color: var(--primary-200);
}

.skill-category::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 4px;
  height: 0;
  background: var(--primary-500);
  transition: height var(--transition-normal) var(--ease-out);
}

.skill-category:hover::before {
  height: 100%;
}

.skill-category h4 {
  color: var(--primary-600);
  margin-bottom: var(--space-lg);
  font-size: var(--text-lg);
  font-weight: var(--font-semibold);
  position: relative;
  display: inline-block;
}

.skill-category h4::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--primary-500);
  transition: width var(--transition-normal) var(--ease-out);
}

.skill-category:hover h4::after {
  width: 100%;
}

.skills-grid {
  display: grid;
  gap: var(--space-lg);
}

.skill-item {
  display: flex;
  flex-direction: column;
  gap: var(--space-xs);
  transition: transform var(--transition-normal) var(--ease-out);
}

.skill-item:hover {
  transform: translateX(var(--space-xs));
}

.skill-name {
  font-size: var(--text-sm);
  color: var(--color-heading);
  font-weight: var(--font-medium);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.skill-name::after {
  content: attr(data-level);
  font-size: var(--text-xs);
  color: var(--neutral-500);
  opacity: 0;
  transition: opacity var(--transition-normal) var(--ease-out);
}

.skill-item:hover .skill-name::after {
  opacity: 1;
}

.skill-bar {
  height: 8px;
  background: var(--neutral-200);
  border-radius: var(--radius-full);
  overflow: hidden;
  position: relative;
}

.skill-level {
  height: 100%;
  background: linear-gradient(90deg, var(--primary-400), var(--primary-600));
  border-radius: var(--radius-full);
  transition: width 1.5s var(--ease-out);
  position: relative;
  overflow: hidden;
}

.skill-level::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    90deg,
    rgba(255, 255, 255, 0) 0%,
    rgba(255, 255, 255, 0.3) 50%,
    rgba(255, 255, 255, 0) 100%
  );
  animation: shimmer 2s infinite;
  transform: translateX(-100%);
}

@keyframes shimmer {
  100% {
    transform: translateX(100%);
  }
}

/* 响应式设计 */
@media (max-width: 768px) {
  .skills-section {
    margin-top: var(--space-xl);
  }

  .skill-category {
    padding: var(--space-md);
  }

  .section-number {
    font-size: var(--text-2xl);
  }
}
</style>
