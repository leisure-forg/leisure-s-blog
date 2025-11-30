<template>
  <section class="contact-section">
    <div class="section-header">
      <div class="section-number">04</div>
      <h3>联系我</h3>
    </div>

    <div class="contact-container">
      <div class="contact-info">
        <h4>让我们保持联系</h4>
        <p>如果您对我的工作感兴趣，或者有任何问题，请随时联系我。我很乐意与您交流！</p>
        
        <div class="contact-methods">
          <div class="contact-method">
            <div class="contact-icon">📧</div>
            <div class="contact-detail">
              <h5>邮箱</h5>
              <p>leisure_667010@163.com</p>
            </div>
          </div>
          
          <div class="contact-method">
            <div class="contact-icon">📱</div>
            <div class="contact-detail">
              <h5>电话</h5>
              <p>+86 182 7910 0239</p>
            </div>
          </div>
          
          <div class="contact-method">
            <div class="contact-icon">📍</div>
            <div class="contact-detail">
              <h5>地址</h5>
              <p>江西省南昌市</p>
            </div>
          </div>
        </div>
        
        <div class="social-links">
          <a href="#" class="social-link">GitHub</a>
          <a href="#" class="social-link">LinkedIn</a>
          <a href="#" class="social-link">微博</a>
        </div>
      </div>
      
      <form class="contact-form" @submit.prevent="handleSubmit">
        <div class="form-group">
          <label for="name">姓名</label>
          <input 
            type="text" 
            id="name" 
            v-model="formData.name" 
            placeholder="请输入您的姓名"
            required
          >
        </div>
        
        <div class="form-group">
          <label for="email">邮箱</label>
          <input 
            type="email" 
            id="email" 
            v-model="formData.email" 
            placeholder="请输入您的邮箱"
            required
          >
        </div>
        
        <div class="form-group">
          <label for="subject">主题</label>
          <input 
            type="text" 
            id="subject" 
            v-model="formData.subject" 
            placeholder="请输入主题"
            required
          >
        </div>
        
        <div class="form-group">
          <label for="message">消息</label>
          <textarea 
            id="message" 
            v-model="formData.message" 
            placeholder="请输入您的消息"
            rows="5"
            required
          ></textarea>
        </div>
        
        <button type="submit" class="submit-btn">发送消息</button>
      </form>
    </div>
  </section>
</template>

<script setup lang="ts">
import { reactive } from 'vue';

const formData = reactive({
  name: '',
  email: '',
  subject: '',
  message: ''
});

const handleSubmit = () => {
  // 这里可以添加表单提交逻辑
  console.log('表单提交数据:', formData);
  alert('感谢您的留言！我会尽快回复您。');
  
  // 重置表单
  formData.name = '';
  formData.email = '';
  formData.subject = '';
  formData.message = '';
};
</script>

<style scoped>
.contact-section {
  margin-top: 4rem;
  margin-bottom: 3rem;
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

.contact-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 3rem;
}

@media (max-width: 768px) {
  .contact-container {
    grid-template-columns: 1fr;
  }
}

.contact-info h4 {
  color: #ff6b35;
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.contact-methods {
  margin-top: 2rem;
}

.contact-method {
  display: flex;
  align-items: center;
  margin-bottom: 1.5rem;
}

.contact-icon {
  font-size: 1.5rem;
  margin-right: 1rem;
  color: #ff6b35;
}

.contact-detail h5 {
  color: #7f8fa4;
  margin-bottom: 0.3rem;
}

.social-links {
  margin-top: 2rem;
  display: flex;
  gap: 1.5rem;
}

.social-link {
  color: #7f8fa4;
  text-decoration: none;
  transition: color 0.3s ease;
}

.social-link:hover {
  color: #ff6b35;
}

.contact-form {
  background: rgba(255, 255, 255, 0.05);
  padding: 2rem;
  border-radius: 10px;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  color: #7f8fa4;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 0.8rem;
  background: rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 5px;
  color: #e6f1ff;
  font-family: inherit;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #ff6b35;
}

.submit-btn {
  background: #ff6b35;
  color: white;
  border: none;
  padding: 1rem 2rem;
  border-radius: 5px;
  cursor: pointer;
  transition:  0.3s ease;
  font-weight: 600;
}

.submit-btn:hover {
  background: #ff4500;
}
</style> 
