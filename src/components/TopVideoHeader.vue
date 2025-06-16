<template>
  <div class="top-video-header">
    <video
      class="video-bg"
      src="../static/video/iorting.mp4"
      autoplay
      muted
      loop
      playsinline
    ></video>
  </div>
</template>

<script setup>
// 暂无逻辑
</script>

<script>
export default {}
</script>

<style scoped>
.top-video-header {
  position: relative;
  width: 100vw; /* 强制宽度占满整个屏幕 */
  height: 50vh; /* 高度为屏幕高度的一半 */
  overflow: hidden; /* 防止视频溢出容器 */
  z-index: 1;
  box-shadow: var(--shadow-lg);
  margin-bottom: var(--space-xl);
}

.video-bg {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform var(--transition-slow) var(--ease-out);
}

/* 添加视频悬停效果 - 轻微放大 */
.top-video-header:hover .video-bg {
  transform: scale(1.05);
}

/* 添加视频蒙版效果 */
.top-video-header::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.2) 0%,
    rgba(0, 0, 0, 0.1) 50%,
    rgba(0, 0, 0, 0.3) 100%
  );
  z-index: 2;
  pointer-events: none;
}
</style>
