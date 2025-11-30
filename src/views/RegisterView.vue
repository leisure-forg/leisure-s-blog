<template>
  <div class="register-container">
    <div class="register-box">
      <h2>注册账号</h2>
      <form @submit.prevent="handleRegister">
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
          <label for="email">邮箱</label>
          <input
            type="email"
            id="email"
            v-model="formData.email"
            placeholder="请输入邮箱"
            required
          />
        </div>

        <div class="form-group">
          <label for="code">验证码</label>
          <div class="code-input-group">
            <input
              type="text"
              id="code"
              v-model="formData.code"
              placeholder="请输入验证码"
              required
            />
            <button type="button" class="send-code-btn" :disabled="cooldown > 0" @click="sendCode">
              {{ cooldown > 0 ? `${cooldown}s后重发` : '发送验证码' }}
            </button>
          </div>
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

        <div class="form-group">
          <label for="confirmPassword">确认密码</label>
          <input
            type="password"
            id="confirmPassword"
            v-model="formData.confirmPassword"
            placeholder="请再次输入密码"
            required
          />
        </div>

        <div class="form-group">
          <label class="agreement">
            <input type="checkbox" v-model="formData.agreement" required />
            我已阅读并同意 <a href="#">服务条款</a> 和 <a href="#">隐私政策</a>
          </label>
        </div>

        <button type="submit" class="register-btn">注册</button>

        <div class="login-link">已有账号？ <router-link to="/login">立即登录</router-link></div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const formData = reactive({
  username: '',
  email: '',
  code: '',
  password: '',
  confirmPassword: '',
  agreement: false,
})

const cooldown = ref(0)
const BASE_URL = 'http://139.196.162.210:8080'

const sendCode = async () => {
  if (!formData.email) {
    alert('请先输入邮箱！')
    return
  }

  try {
    const response = await fetch(`${BASE_URL}/api/v1/auth/register/send_code`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email: formData.email }),
    })

    if (response.ok) {
      alert('验证码已发送，请查收邮件！')
      cooldown.value = 60
      const timer = setInterval(() => {
        cooldown.value--
        if (cooldown.value <= 0) clearInterval(timer)
      }, 1000)
    } else {
      alert('发送验证码失败，请稍后重试')
    }
  } catch (error) {
    console.error('Send code error:', error)
    alert('发送请求出错')
  }
}

const handleRegister = async () => {
  if (formData.password !== formData.confirmPassword) {
    alert('两次输入的密码不一致！')
    return
  }

  try {
    const response = await fetch(`${BASE_URL}/api/v1/auth/register`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        username: formData.username,
        password: formData.password,
        email: formData.email,
        code: formData.code,
      }),
    })

    if (response.ok) {
      alert('注册成功！请登录')
      router.push('/login')
    } else {
      alert('注册失败，请检查验证码或稍后重试')
    }
  } catch (error) {
    console.error('Register error:', error)
    alert('注册请求出错')
  }
}
</script>

<style scoped>
.register-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
}

.register-box {
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
input[type='email'],
input[type='password'] {
  width: 100%;
  padding: 0.8rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
}

input[type='text']:focus,
input[type='email']:focus,
input[type='password']:focus {
  outline: none;
  border-color: #ff6b35;
}

.agreement {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.9rem;
}

.agreement a {
  color: #ff6b35;
  text-decoration: none;
}

.agreement a:hover {
  text-decoration: underline;
}

.register-btn {
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

.register-btn:hover {
  background: #ff4500;
}

.login-link {
  text-align: center;
  margin-top: 1.5rem;
  color: #666;
}

.login-link a {
  color: #ff6b35;
  text-decoration: none;
}

.login-link a:hover {
  text-decoration: underline;
}

.code-input-group {
  display: flex;
  gap: 0.5rem;
}

.code-input-group input {
  flex: 1;
}

.send-code-btn {
  padding: 0 1rem;
  background: rgb(247, 127, 0);
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 0.9rem;
  white-space: nowrap;
  transition: background-color 0.3s;
}

.send-code-btn:hover:not(:disabled) {
  background: #f9612a;
}

.send-code-btn:disabled {
  background: #ccc;
  cursor: not-allowed;
}
</style>
