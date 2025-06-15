<template>
  <div class="main-content-area">
    
    <section class="recommend">
      <h2>推荐文章</h2>
      <div class="article-card">
        <div class="article-title">redis解决缓存击穿、缓存穿透方法…</div>
        <div class="article-meta">
          2025-05-09 15:53:59 | <span>SpringBoot</span> <span>redis</span>
        </div>
        <div class="article-desc">解决redis的缓存击穿和穿透问题是笔者的体会和理解…</div>
        <div class="article-actions">
          <span>👍 2</span>
          <span>💬 0</span>
          <span>👁️ 85</span>
        </div>
      </div>
    </section>
    <section class="recent-update">
      <h2>最近更新</h2>
      <div class="update-card">
        <img
          class="update-img"
          src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=400&q=80"
          alt="更新图片"
        />
        <div class="update-info">
          <div class="update-title">聊一聊最近在干什么…</div>
          <div class="update-meta">2025-05-20 20:10:08 | 最近状态</div>
          <div class="update-desc">最近技术又摸索了不少，聊聊最近在干的事情吧…</div>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
// 暂无逻辑
</script>

<script>
export default {}
</script>

<style scoped>
.main-content-area {
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  padding: 32px 24px;
  width: 100%;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
}

.weather-info {
  color: #333;
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 24px;
  padding: 16px 32px;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 16px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
  letter-spacing: 2px;
}

.recommend,
.recent-update {
  margin-bottom: 32px;
}

.article-card,
.update-card {
  background: #f8faff;
  border-radius: 12px;
  padding: 20px;
  margin-top: 12px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.12);
}

.article-title {
  font-weight: bold;
  font-size: 1.1rem;
  margin-bottom: 8px;
}

.article-meta {
  color: #888;
  font-size: 0.95rem;
  margin-bottom: 8px;
}

.article-meta span {
  background: #e3f0ff;
  color: #409eff;
  border-radius: 8px;
  padding: 2px 8px;
  margin-right: 6px;
  font-size: 0.9em;
}

.article-desc {
  color: #555;
  margin-bottom: 10px;
}

.article-actions span {
  margin-right: 16px;
  color: #888;
  font-size: 0.95em;
}

.update-card {
  display: flex;
  align-items: flex-start;
}

.update-img {
  width: 100px;
  height: 70px;
  object-fit: cover;
  border-radius: 8px;
  margin-right: 16px;
}

.update-info {
  flex: 1;
}

.update-title {
  font-weight: bold;
  margin-bottom: 6px;
}

.update-meta {
  color: #888;
  font-size: 0.95rem;
  margin-bottom: 6px;
}

.update-desc {
  color: #555;
}
</style>
