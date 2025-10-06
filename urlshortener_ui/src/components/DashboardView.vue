<template>
  <div class="p-8">
    <h1 class="text-3xl font-bold mb-4">Welcome to your Dashboard 🎉</h1>

    <div v-if="userData">
      <p><strong>Email:</strong> {{ userData.email }}</p>
    </div>

    <button 
      @click="logout" 
      class="bg-red-600 text-white px-4 py-2 rounded hover:bg-red-700 mt-6">
      Logout
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
// import { authFetch } from '../authFetch.js'
import { useRouter } from 'vue-router'

const router = useRouter()
const userData = ref(null)

onMounted(async () => {
  const token = localStorage.getItem('token')
  if (!token) {
    router.push('/login')
    return
  }

  try {
    const response = await fetch('http://localhost:5000/api/me', {
      headers: {
        'Authorization': `Bearer ${token}`,
      },
    })

    if (response.ok) {
      userData.value = await response.json()
    } else {
      // invalid token or server error — redirect to login
      localStorage.removeItem('token')
      router.push('/login')
    }
  } catch (err) {
    console.error('Failed to fetch user data', err)
    localStorage.removeItem('token')
    router.push('/login')
  }
})

const logout = () => {
  localStorage.removeItem('token')
  router.push('/login')
}
</script>
