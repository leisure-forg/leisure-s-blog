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
export default {};
</script>

<style scoped>
.top-video-header {
  position: relative;
  width: 100vw;  /* 强制宽度占满整个屏幕 */
  height: 50vh;  /* 高度为屏幕高度的一半 */
  overflow: hidden;  /* 防止视频溢出容器 */
  z-index: 1;
}

.video-bg {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style> 