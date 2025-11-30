<template>
  <div class="sidebar-profile">
    <router-link to="/about">
      <img
        class="avatar"
        src="../static/picture/evilsteel_girl_sitting_in_cafe_at_night_alone_watching_outside_32_9_5120X1440.png"
        alt="头像"
      />
    </router-link>
    <div class="nickname">Leisure</div>
    <div class="desc">一个热爱前端开发的大学生，喜欢折腾各种技术，偶尔玩玩3D游戏。</div>
    <div class="contacts">
      <i title="邮箱: xu667010@163.com">
        <img src="../static/picture/email.jpg" alt="邮箱" class="contact-icon" />
      </i>
      <i title="gitgub:https://github.com/leisure-forg">
        <img src="../static/picture/github.png" alt="GitHub" class="contact-icon" />
      </i>
      <i title="QQ: 1015566918">
        <img src="../static/picture/qq.png" alt="QQ" class="contact-icon" />
      </i>
    </div>
  </div>
</template>

<script setup lang="ts">
// 暂无逻辑
</script>

<style scoped>
.sidebar-profile {
  background: var(--color-background-soft);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-md);
  padding: var(--space-xl) var(--space-md) var(--space-lg);
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow-y: auto;
  transition: box-shadow var(--transition-normal) var(--ease-out);
}

.sidebar-profile:hover {
  box-shadow: 5px 5px 15px var(--color-shadow);
}

.avatar {
  width: 100px;
  height: 100px;
  border-radius: var(--radius-full);
  object-fit: cover;
  margin-bottom: var(--space-md);
  border: 3px solid var(--color-border-accent);
  transition: all var(--transition-normal) var(--ease-out);
}

.avatar:hover {
  transform: scale(1.1);
  border-color: var(--color-primary);
  box-shadow: var(--shadow-lg);
}

.nickname {
  font-size: var(--text-xl);
  font-weight: var(--font-bold);
  margin-bottom: var(--space-xs);
  color: var(--color-heading);
}

.desc {
  font-size: var(--text-sm);
  color: var(--color-text-soft);
  text-align: center;
  margin-bottom: var(--space-md);
  line-height: 1.6;
}

.contacts {
  display: flex;
  gap: var(--space-md);
  align-items: center;
}

.contacts i {
  text-decoration: none;
  transition: all var(--transition-normal) var(--ease-out);
  display: inline-block;
  position: relative;
  cursor: pointer;
}

.contacts i:hover {
  transform: scale(1.15);
}

.contact-icon {
  width: 24px;
  height: 24px;
  object-fit: contain;
  opacity: 0.8;
  transition: opacity var(--transition-normal) var(--ease-out);
}

.contacts i:hover .contact-icon {
  opacity: 1;
}

.contacts i::after {
  content: attr(title);
  position: absolute;
  bottom: 120%;
  left: 50%;
  transform: translateX(-50%) scale(0.9);
  padding: var(--space-xs) var(--space-sm);
  background-color: var(--color-background);
  color: var(--color-text);
  border-radius: var(--radius-sm);
  font-size: var(--text-xs);
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: all var(--transition-normal) var(--ease-out);
  box-shadow: var(--shadow-sm);
  pointer-events: none;
  z-index: 10;
}

.contacts i::before {
  content: '';
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  border-width: 5px;
  border-style: solid;
  border-color: var(--color-background) transparent transparent transparent;
  opacity: 0;
  visibility: hidden;
  transition: all var(--transition-normal) var(--ease-out);
  z-index: 11;
}

.contacts i:hover::after,
.contacts i:hover::before {
  opacity: 1;
  visibility: visible;
  transform: translateX(-50%) scale(1);
}
</style>
