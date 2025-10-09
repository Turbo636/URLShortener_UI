<template>
  <div class="container">
    <div class="dashboard-container">
      <div class=" flex items-center justify-center p-6">
        <div class="bg-white w-full max-w-3xl rounded-2xl shadow-lg p-8">
          
          <!-- Logout Button -->
          <div class="information-container">
            <div>
              <h1 class="text-3xl font-bold text-center mb-8 text-gray-800">
                Welcome to your Dashboard 🎉
              </h1>
            </div>
            
            <button 
              @click="logout" 
              class="logoutBtn">
              Logout
            </button>
          </div>
          
          <!-- User Information -->
          <div class="bg-gray-50 rounded-lg shadow-inner p-6 mb-8">
            <h2 class="text-xl font-semibold mb-4 text-gray-700 text-center">
              User Information
            </h2>
            <div class="space-y-3 text-center">
              <p class="text-gray-600"><strong class="text-gray-800">Name:</strong> asdf</p>
              <p class="text-gray-600"><strong class="text-gray-800">Surname:</strong> asdf</p>
              <p class="text-gray-600"><strong class="text-gray-800">Email:</strong> asdf</p>
              <p class="text-gray-600"><strong class="text-gray-800">Wallet Balance:</strong> asdf</p>
            </div>
          </div>

          <!-- Quick Actions -->
          <div class="bg-gray-50 rounded-lg shadow-inner p-6">
            <h2 class="text-xl font-semibold mb-6 text-gray-700 text-center">
              Quick Actions
            </h2>
            <div class="actions-grid">
              <div class="urlCreation">
                <label class="block text-gray-700 mb-2 font-medium">Create new URL:</label>
                <button class="w-full bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700 transition">
                  Create URL
                </button>
              </div>
              <div class="viewWallet">
                <label class="block text-gray-700 mb-2 font-medium">View Clicks:</label>
                <button class="w-full bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700 transition">
                  View Clicks
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>  


  
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const userData = ref('')

onMounted(() => {
  console.log('DashboardView mounted')
  console.log(userData.value)
})

const logout = () => {
  localStorage.removeItem('token')
  router.push('/login')
}
</script>

<style scoped>
/* Optional subtle gradient for background */
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: #2c3e50;
}


.dashboard-container {
  background: white;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  width: 80%;
  height: 80vh;
  max-width: 90%;
}

.information-container {
  display: flex;
  justify-content: space-between; /* pushes header left and button right */
  align-items: center;            /* vertically center both */
  margin-bottom: 1.5rem;
  h1 {

    width: 100%;
    margin: 0; /* remove default margin */
    
  }
}

.logoutBtn {
  background-color: #e53e3e; /* Red-600 */
  color: white;
  padding: 0.5rem 1.5rem;
  border-radius: 0.375rem;
  transition: background-color 0.3s;
}

.actions-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
}

.urlCreation, .viewWallet {
  display: grid;
  align-items: center;
justify-content: center;
  button {
    margin-top: 0.5rem;
    
  }
}


</style>
