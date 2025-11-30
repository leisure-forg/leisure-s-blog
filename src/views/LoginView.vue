<template>
  <div class="login-container">
    <div class="login-box">
      <h2>登录</h2>
      <form @submit.prevent="handleLogin">
        <div class="form-group">
          <label for="username">用户名</label>
          <input
            type="text"
            id="username"
            v-model="formData.username"
            placeholder="请输入用户名"
            required
          />
        </div>

        <div class="form-group">
          <label for="password">密码</label>
          <input
            type="password"
            id="password"
            v-model="formData.password"
            placeholder="请输入密码"
            required
          />
        </div>

        <div class="form-options">
          <label class="remember-me">
            <input type="checkbox" v-model="formData.remember" />
            记住我
          </label>
          <a href="#" class="forgot-password">忘记密码？</a>
        </div>

        <button type="submit" class="login-btn" v-if="!isGuestMode">登录</button>
        <button type="button" class="guest-btn" @click="handleGuestLogin">
          {{ isGuestMode ? '确认游客登录' : '游客登录' }}
        </button>
        <button type="button" class="return-btn" v-if="isGuestMode" @click="handleReturn">
          返回
        </button>

        <div class="register-link">
          还没有账号？ <router-link to="/register">立即注册</router-link>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const isGuestMode = ref(false)
const formData = reactive({
  username: '',
  password: '',
  remember: false,
})

const BASE_URL = 'http://139.196.162.210:8080'

const handleLogin = async () => {
  try {
    const response = await fetch(`${BASE_URL}/api/v1/auth/login`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        username: formData.username,
        password: formData.password,
      }),
    })

    if (response.ok) {
      const data = await response.json()
      localStorage.setItem('token', data.token)
      localStorage.setItem('isLoggedIn', 'true')
      router.push('/')
    } else {
      alert('登录失败，请检查用户名或密码！')
    }
  } catch (error) {
    console.error('Login error:', error)
    alert('登录请求出错，请稍后重试')
  }
}

const handleGuestLogin = async () => {
  if (!isGuestMode.value) {
    // 第一步：生成随机账号密码并填充
    const randomUser = 'Guest_' + Math.random().toString(36).slice(-6)
    const randomPass = Math.random().toString(36).slice(-8)

    formData.username = randomUser
    formData.password = randomPass
    isGuestMode.value = true
    return
  }

  // 第二步：执行游客登录
  const guestUser = {
    username: formData.username,
    password: formData.password,
  }

  try {
    const response = await fetch(`${BASE_URL}/api/v1/auth/guest_login`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(guestUser),
    })

    if (response.ok) {
      const data = await response.json()
      localStorage.setItem('token', data.token)
      localStorage.setItem('isLoggedIn', 'true')
      alert(`游客登录成功！\n账号: ${guestUser.username}\n密码: ${guestUser.password}`)
      router.push('/')
    } else {
      alert('游客登录失败')
    }
  } catch (error) {
    console.error('Guest login error:', error)
    alert('请求出错')
  }
}

const handleReturn = () => {
  isGuestMode.value = false
  formData.username = ''
  formData.password = ''
}
</script>

<style scoped>
.login-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.login-box {
  background: white;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
}

h2 {
  text-align: center;
  color: #333;
  margin-bottom: 2rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  color: #666;
}

input[type='text'],
input[type='password'] {
  width: 100%;
  padding: 0.8rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
}

input[type='text']:focus,
input[type='password']:focus {
  outline: none;
  border-color: #ff6b35;
}

.form-options {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
}

.remember-me {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: #666;
}

.forgot-password {
  color: #ff6b35;
  text-decoration: none;
}

.login-btn {
  width: 100%;
  padding: 0.8rem;
  background: #ff6b35;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.login-btn:hover {
  background: #ff4500;
}

.guest-btn {
  width: 100%;
  padding: 0.8rem;
  background: #6c757d;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  margin-top: 1rem;
  transition: background-color 0.3s;
}

.guest-btn:hover {
  background: #5a6268;
}

.register-link {
  text-align: center;
  margin-top: 1.5rem;
  color: #666;
}

.register-link a {
  color: #ff6b35;
  text-decoration: none;
}

.register-link a:hover {
  text-decoration: underline;
}

.return-btn {
  width: 100%;
  padding: 0.8rem;
  background: transparent;
  color: #666;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  margin-top: 1rem;
  transition: all 0.3s;
}

.return-btn:hover {
  background: #f5f5f5;
  border-color: #ccc;
}
</style>
