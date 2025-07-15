<template>
  <div class="login">
    <h2>用户登录</h2>
    <input v-model="form.username" placeholder="用户名" />
    <input v-model="form.password" type="password" placeholder="密码" />
    <button @click="login">登录</button>
    <p>{{ message }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const form = ref({
  username: '',
  password: ''
})
const message = ref('')

async function login() {
  const res = await fetch('/api/customer/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(form.value)
  })
  const text = await res.text()
  message.value = text
  if (text === '登录成功') {
    // 假设用户ID是 1，实际应通过 token 或登录返回值动态设置
    router.push(`/profile/1`)
  }
}
</script>
