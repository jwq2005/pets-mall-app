<template>
  <div class="register">
    <h2>用户注册</h2>
    <input v-model="form.username" placeholder="用户名" />
    <input v-model="form.password" type="password" placeholder="密码" />
    <input v-model="form.name" placeholder="姓名" />
    <input v-model="form.phone" placeholder="手机号" />
    <input v-model="form.email" placeholder="邮箱" />
    <button @click="register">注册</button>
    <p>{{ message }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const form = ref({
  username: '',
  password: '',
  name: '',
  phone: '',
  email: ''
})
const message = ref('')

async function register() {
  const res = await fetch('/api/customer/register', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(form.value)
  })
  const text = await res.text()
  message.value = text
  if (text === '注册成功') {
    router.push('/login')
  }
}
</script>
