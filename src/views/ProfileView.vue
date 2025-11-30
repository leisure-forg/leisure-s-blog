<template>
  <div class="profile-container">
    <div class="profile-header">
      <h2>个人资料</h2>
      <p>完善您的个人信息，让其他用户更好地了解您</p>
    </div>
    
    <div class="profile-content">
      <div class="avatar-section">
        <div class="avatar-wrapper">
          <img :src="userAvatar" alt="用户头像" class="avatar">
          <div class="avatar-upload">
            <label for="avatar-upload" class="upload-btn">
              <span class="icon">📷</span>
              更换头像
            </label>
            <input type="file" id="avatar-upload" accept="image/*" @change="handleAvatarChange" hidden>
          </div>
        </div>
      </div>
      
      <form class="profile-form" @submit.prevent="handleSubmit">
        <div class="form-row">
          <div class="form-group">
            <label for="username">用户名</label>
            <input 
              type="text" 
              id="username" 
              v-model="formData.username" 
              placeholder="请输入用户名"
              required
            >
          </div>
          
          <div class="form-group">
            <label for="email">邮箱</label>
            <input 
              type="email" 
              id="email" 
              v-model="formData.email" 
              placeholder="请输入邮箱"
              required
            >
          </div>
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <label for="phone">手机号码</label>
            <input 
              type="tel" 
              id="phone" 
              v-model="formData.phone" 
              placeholder="请输入手机号码"
            >
          </div>
          
          <div class="form-group">
            <label for="age">年龄</label>
            <input 
              type="number" 
              id="age" 
              v-model="formData.age" 
              placeholder="请输入年龄"
              min="1"
              max="120"
            >
          </div>
        </div>
        
        <div class="form-group">
          <label for="bio">个性签名</label>
          <textarea 
            id="bio" 
            v-model="formData.bio" 
            placeholder="介绍一下自己吧..."
            rows="4"
          ></textarea>
        </div>
        
        <div class="form-group">
          <label for="location">所在地</label>
          <input 
            type="text" 
            id="location" 
            v-model="formData.location" 
            placeholder="请输入所在地"
          >
        </div>
        
        <div class="form-actions">
          <button type="submit" class="save-btn">保存修改</button>
          <button type="button" class="cancel-btn" @click="handleCancel">取消</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const userAvatar = ref('https://api.dicebear.com/7.x/avataaars/svg?seed=admin');

const formData = reactive({
  username: 'admin',
  email: 'leisure_667010@163.com',
  phone: '',
  age: '',
  bio: '',
  location: ''
});

const handleAvatarChange = (event: Event) => {
  const input = event.target as HTMLInputElement
  const file = input.files?.[0]
  if (file) {
    // 这里可以添加上传头像的逻辑
    alert('头像上传功能暂未实现！');
  }
};

const handleSubmit = () => {
  // 这里可以添加保存个人资料的逻辑
  alert('个人资料保存成功！');
  router.push('/');
};

const handleCancel = () => {
  router.push('/');
};
</script>

<style scoped>
.profile-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

.profile-header {
  text-align: center;
  margin-bottom: 3rem;
}

.profile-header h2 {
  color: #333;
  margin-bottom: 0.5rem;
}

.profile-header p {
  color: #666;
}

.profile-content {
  background: white;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  padding: 2rem;
}

.avatar-section {
  display: flex;
  justify-content: center;
  margin-bottom: 2rem;
}

.avatar-wrapper {
  text-align: center;
}

.avatar {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  border: 3px solid #ff6b35;
  margin-bottom: 1rem;
}

.avatar-upload {
  margin-top: 1rem;
}

.upload-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  background: #f5f5f5;
  border-radius: 20px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.upload-btn:hover {
  background: #e0e0e0;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  margin-bottom: 1.5rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  color: #666;
}

input[type="text"],
input[type="email"],
input[type="tel"],
input[type="number"],
textarea {
  width: 100%;
  padding: 0.8rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #ff6b35;
}

.form-actions {
  display: flex;
  gap: 1rem;
  margin-top: 2rem;
}

.save-btn,
.cancel-btn {
  padding: 0.8rem 2rem;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.save-btn {
  background: #ff6b35;
  color: white;
  border: none;
}

.save-btn:hover {
  background: #ff4500;
}

.cancel-btn {
  background: #f5f5f5;
  color: #666;
  border: 1px solid #ddd;
}

.cancel-btn:hover {
  background: #e0e0e0;
}

@media (max-width: 768px) {
  .form-row {
    grid-template-columns: 1fr;
  }
  
  .profile-container {
    padding: 1rem;
  }
}
</style> 
