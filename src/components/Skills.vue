<template>
  <section class="skills-section">
    <div class="section-header">
      <div class="section-number">02</div>
      <h3>技能专长</h3>
    </div>

    <div class="skills-container">
      <div class="skill-category">
        <h4>后端开发</h4>
        <div class="skills-grid">
          <div class="skill-item">
            <div class="skill-name">Java</div>
            <div class="skill-bar">
              <div class="skill-level" style="width: 90%"></div>
            </div>
          </div>
          <div class="skill-item">
            <div class="skill-name">Mysql</div>
            <div class="skill-bar">
              <div class="skill-level" style="width: 85%"></div>
            </div>
          </div>
          <div class="skill-item">
            <div class="skill-name">Vue.js</div>
            <div class="skill-bar">
              <div class="skill-level" style="width: 80%"></div>
            </div>
          </div>
        </div>
      </div>

      <div class="skill-category">
        <h4>工具和框架</h4>
        <div class="skills-grid">
          <div class="skill-item">
            <div class="skill-name">Springboot</div>
            <div class="skill-bar">
              <div class="skill-level" style="width: 70%"></div>
            </div>
          </div>
          <div class="skill-item">
            <div class="skill-name">git</div>
            <div class="skill-bar">
              <div class="skill-level" style="width: 65%"></div>
            </div>
          </div>
          <div class="skill-item">
            <div class="skill-name">Docker</div>
            <div class="skill-bar">
              <div class="skill-level" style="width: 60%"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.skills-section {
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

.skills-container {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.skill-category {
  background: rgba(255, 255, 255, 0.05);
  padding: 2rem;
  border-radius: 10px;
}

.skill-category h4 {
  color: #ff6b35;
  margin-bottom: 1.5rem;
  font-size: 1.2rem;
}

.skills-grid {
  display: grid;
  gap: 1.5rem;
}

.skill-item {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.skill-name {
  font-size: 0.9rem;
  color: #121213;
}

.skill-bar {
  height: 6px;
  background: rgba(184, 182, 182, 0.1);
  border-radius: 3px;
  overflow: hidden;
}

.skill-level {
  height: 100%;
  background: #ff6b35;
  border-radius: 3px;
  transition: width 1s ease-in-out;
}
</style> 