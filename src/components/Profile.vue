<template>
  <div>
    <h2>个人信息</h2>
    <p>姓名：{{ profile.name }}</p>
    <p>手机号：{{ profile.phone }}</p>
    <p>邮箱：{{ profile.email }}</p>
    <button @click="goToEdit">修改信息</button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const profile = ref({})
const route = useRoute()
const router = useRouter()
const id = route.params.id

onMounted(async () => {
  const res = await fetch(`/api/customer/${id}`)
  profile.value = await res.json()
})

function goToEdit() {
  router.push(`/edit/${id}`)
}
</script>
