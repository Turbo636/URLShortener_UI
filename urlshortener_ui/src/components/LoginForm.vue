<template>
  <div class="flex h-screen justify-center items-center bg-gray-100">
    <div class="bg-white p-8 rounded shadow-md w-96">
      <h2 class="text-2xl font-bold mb-6 text-center">Login</h2>
      <form @submit.prevent="login">
        <div class="mb-4">
          <label class="block mb-1 text-gray-700">Email</label>
          <input v-model="email" type="email" class="w-full p-2 border rounded" required />
        </div>
        <div class="mb-4">
          <label class="block mb-1 text-gray-700">Password</label>
          <input v-model="password" type="password" class="w-full p-2 border rounded" required />
        </div>
        <button type="submit" class="w-full bg-blue-600 text-white py-2 rounded hover:bg-blue-700">
          Login
        </button>
        <p v-if="error" class="text-red-500 mt-3 text-center">{{ error }}</p>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
// import { useRouter } from 'vue-router'

// const router = useRouter()
const email = ref('')
const password = ref('')
const error = ref('')

const login = async () => {
  error.value = ''
  try {
    const response = await fetch('http://localhost:5000/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        email: email.value,
        password: password.value,
      }),
    })

    if (!response.ok) {
      const errData = await response.json()
      throw new Error(errData.message || 'Login failed')
    }

    const data = await response.json()
    localStorage.setItem('token', data.token)

    // router.push('/dashboard')
  } catch (err) {
    error.value = err.message
  }
}
</script>

<style scoped>
body {
  font-family: 'Inter', sans-serif;
}
</style>
