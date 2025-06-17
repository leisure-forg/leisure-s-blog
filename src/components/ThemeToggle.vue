<template>
  <div class="theme-toggle">
    <button
      class="toggle-button"
      @click="toggleTheme"
      :title="currentTheme === 'default' ? '切换到蓝紫主题' : '切换到橙色主题'"
    >
      <div class="toggle-track" :class="{ 'is-indigo': currentTheme === 'indigo' }">
        <div class="toggle-thumb">
          <div class="icon-container">
            <!-- 橙色主题图标 -->
            <svg
              v-show="currentTheme === 'default'"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="currentColor"
              class="theme-icon"
            >
              <path
                d="M12 2.25a.75.75 0 01.75.75v2.25a.75.75 0 01-1.5 0V3a.75.75 0 01.75-.75zM7.5 12a4.5 4.5 0 119 0 4.5 4.5 0 01-9 0zM18.894 6.166a.75.75 0 00-1.06-1.06l-1.591 1.59a.75.75 0 101.06 1.061l1.591-1.59zM21.75 12a.75.75 0 01-.75.75h-2.25a.75.75 0 010-1.5H21a.75.75 0 01.75.75zM17.834 18.894a.75.75 0 001.06-1.06l-1.59-1.591a.75.75 0 10-1.061 1.06l1.59 1.591zM12 18a.75.75 0 01.75.75V21a.75.75 0 01-1.5 0v-2.25A.75.75 0 0112 18zM7.758 17.303a.75.75 0 00-1.061-1.06l-1.591 1.59a.75.75 0 001.06 1.061l1.591-1.59zM6 12a.75.75 0 01-.75.75H3a.75.75 0 010-1.5h2.25A.75.75 0 016 12zM6.697 7.757a.75.75 0 001.06-1.06l-1.59-1.591a.75.75 0 00-1.061 1.06l1.59 1.591z"
              />
            </svg>

            <!-- 蓝紫主题图标 -->
            <svg
              v-show="currentTheme === 'indigo'"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="currentColor"
              class="theme-icon"
            >
              <path
                fill-rule="evenodd"
                d="M9.528 1.718a.75.75 0 01.162.819A8.97 8.97 0 009 6a9 9 0 009 9 8.97 8.97 0 003.463-.69.75.75 0 01.981.98 10.503 10.503 0 01-9.694 6.46c-5.799 0-10.5-4.701-10.5-10.5 0-4.368 2.667-8.112 6.46-9.694a.75.75 0 01.818.162z"
                clip-rule="evenodd"
              />
            </svg>
          </div>
        </div>
      </div>
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

// 从 localStorage 初始化主题，如果没有保存过主题则默认使用 default
const savedTheme = localStorage.getItem('user-theme') || 'default'
const currentTheme = ref(savedTheme)

// 切换主题
const toggleTheme = () => {
  const newTheme = currentTheme.value === 'default' ? 'indigo' : 'default'
  currentTheme.value = newTheme
  applyTheme(newTheme)
  console.log('Theme toggled to:', newTheme) // 调试日志
}

// 应用主题到 HTML 元素
const applyTheme = (theme) => {
  console.log('Applying theme:', theme)

  // 确保在切换主题之前移除现有的主题
  document.documentElement.removeAttribute('data-theme')

  const root = document.documentElement
  if (theme === 'indigo') {
    root.setAttribute('data-theme', 'indigo')
    // 设置深色主题的背景色和文本颜色
    root.style.setProperty('--color-background', '#1a1a2e')
    root.style.setProperty('--color-background-soft', '#242438')
    root.style.setProperty('--color-background-mute', '#2f2f46')
    root.style.setProperty('--color-text', '#e1e1ff')
    root.style.setProperty('--color-text-soft', '#a8a8d0')
  } else {
    // 设置默认（橙色）主题的背景色和文本颜色
    root.style.setProperty('--color-background', '#ffffff')
    root.style.setProperty('--color-background-soft', '#f8f8f8')
    root.style.setProperty('--color-background-mute', '#f2f2f2')
    root.style.setProperty('--color-text', '#2c3e50')
    root.style.setProperty('--color-text-soft', '#4a5568')
  }

  // 保存主题到 localStorage
  localStorage.setItem('user-theme', theme)

  // 验证主题是否正确应用
  console.log('Current data-theme attribute:', root.getAttribute('data-theme'))
}

// 监听主题变化
watch(currentTheme, (newTheme) => {
  console.log('Theme changed in watcher:', newTheme) // 调试日志
  applyTheme(newTheme)
})

// 组件挂载时，立即应用当前主题
onMounted(() => {
  console.log('Component mounted, applying theme:', currentTheme.value)
  applyTheme(currentTheme.value)

  // 监听系统主题变化
  window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
    if (!localStorage.getItem('user-theme')) {
      const newTheme = e.matches ? 'indigo' : 'default'
      currentTheme.value = newTheme
      applyTheme(newTheme)
    }
  })
})
</script>

<style scoped>
.theme-toggle {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 1rem 0;
}

.toggle-button {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  outline: none;
}

.toggle-track {
  width: 50px;
  height: 24px;
  background-color: var(--accent-500);
  border-radius: 12px;
  position: relative;
  transition: background-color 0.3s ease;
  display: flex;
  align-items: center;
  padding: 2px;
}

.toggle-track.is-indigo {
  background-color: var(--theme-indigo-500);
}

.toggle-thumb {
  width: 20px;
  height: 20px;
  background-color: white;
  border-radius: 50%;
  position: relative;
  transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
}

.toggle-track.is-indigo .toggle-thumb {
  transform: translateX(26px);
}

.icon-container {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  color: var(--accent-500);
}

.toggle-track.is-indigo .icon-container {
  color: var(--theme-indigo-500);
}

.theme-icon {
  width: 12px;
  height: 12px;
}
</style>
