<template>
  <div class="sidebar-profile">
    <img class="avatar" src="../static/picture/evilsteel_girl_sitting_in_cafe_at_night_alone_watching_outside_32_9_5120X1440.png" alt="头像" />
    <div class="nickname">Leisure</div>
    <div class="desc">一个热爱前端开发的大学生，喜欢折腾各种技术，偶尔玩玩3D游戏。</div>
    <div class="contacts">
      <a href="mailto:example@email.com" title="邮箱"><i class="iconfont">✉️</i></a>
      <a href="https://github.com/" target="_blank" title="GitHub"><i class="iconfont">🐱</i></a>
      <a href="https://qm.qq.com/" target="_blank" title="QQ"><i class="iconfont">💬</i></a>
    </div>
  </div>
</template>

<script setup>
// 暂无逻辑
</script>

<script>
export default {}
</script>

<style scoped>
.sidebar-profile {
  background: #fff;
  border-radius: 16px;
  box-shadow: 3px 2px 8px rgba(0, 0, 0, 0.04);
  padding: 32px 16px 24px 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow-y: auto;
}
.avatar {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 16px;
  border: 3px solid #f0f0f0;
}
.nickname {
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 8px;
}
.desc {
  font-size: 0.95rem;
  color: #888;
  text-align: center;
  margin-bottom: 16px;
}
.contacts a {
  margin: 0 8px;
  font-size: 1.3rem;
  color: #333;
  text-decoration: none;
  transition: color 0.2s;
}
.contacts a:hover {
  color: #409eff;
}
</style>
