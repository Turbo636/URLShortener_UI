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

            <div>
              <p class="text-gray-600"><strong class="text-gray-800">Wallet Balance:</strong> asdf</p>
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
              
            </div>
          </div>

          <!-- Quick Actions -->
          <div class="bg-gray-50 rounded-lg shadow-inner p-6">
            <h2 class="text-xl font-semibold mb-6 text-gray-700 text-center">
              Quick Actions
            </h2>
            <div class="actions-grid">
              <div class="urlCreation">
                <!-- <label class="block text-gray-700 mb-2 font-medium">Create new URL:</label> -->
                <button class="dashboardBtns" @click="showCreateUrlForm">
                  Create URL
                </button>
              </div>
              <div class="viewWallet">
                <!-- <label class="block text-gray-700 mb-2 font-medium">View Clicks:</label> -->
                <button class="dashboardBtns" @click="showClicksTable">
                  View Clicks
                </button>
              </div>
            </div>
              <div class="actions">
                <div class="mb-8" v-if="newUrlRequired">
                  <h3 class="text-lg font-medium mb-3 text-gray-700">Create a New Short URL</h3>
                  <form @submit.prevent="createUrl" class="create-url">
                    <input
                      v-model="newUrl"
                      type="text"
                      placeholder="Enter your long URL"
                    />
                    <button class="dashboardBtns" type="submit">
                      Submit
                    </button>
                  </form>
                  <div class="urlResponse" v-if="showUrlCreated">
                    <label >New URL Created:</label>
  
                    <!-- Display the new short URL -->
                    <span class="newUrlCreated"><b>{{url}}</b></span>

                  </div>
                  
                </div>
                <div v-if="showTableClicks" class="tableClick mt-8">
                  <h3 class="text-lg font-medium mb-3 text-gray-700">URL Click Statistics</h3>
                  <table >
                    <thead class="bg-gray-100">
                      <tr>
                        <th class="border border-gray-200 px-3 py-2 text-left">Short URL</th>
                        <th class="border border-gray-200 px-3 py-2 text-left">Original URL</th>
                        <th class="border border-gray-200 px-3 py-2 text-center">Clicks</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="(item, index) in clickData" :key="index">
                        <td >
                          {{ item.shortUrl }}
                        </td>
                        <td >
                          {{ item.originalUrl }}
                        </td>
                        <td >
                          {{ item.clicks }}
                        </td>
                      </tr>

                      <tr v-if="clickData.length === 0">
                        <td colspan="3" >
                          No data found
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <!-- </div> -->
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
const newUrl = ref('')
const newUrlRequired = ref(false)
const showTableClicks = ref(false)
const showUrlCreated = ref(false)
const url = ref('')


const showCreateUrlForm = () => {
  newUrlRequired.value = true
  showTableClicks.value = false
  showUrlCreated.value = false
}

const showClicksTable = () => {
  showTableClicks.value = true
  newUrlRequired.value = false
  showUrlCreated.value = false
}

const createUrl = () => {
  if (!newUrl.value) return alert('Please enter a URL')

  // Here you would typically send newUrl.value to your backend to create the short URL
  console.log('Creating short URL for:', newUrl.value)

  // Reset input field after submission
  newUrl.value = ''
  showUrlCreated.value = true
  url.value = 'https://localhost:3232/WWci3' // This should be set to the actual created URL from backend
}

const clickData = ref([
  { shortUrl: 'abc123', originalUrl: 'https://example.com/product/123', clicks: 12 },
  { shortUrl: 'xyz789', originalUrl: 'https://mywebsite.com/page', clicks: 5 }
])

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
    padding: 0.5rem 1.5rem;
    border-radius: 0.375rem;
    color: white;
    cursor: pointer;
    background: linear-gradient(135deg, #1e3ec8 0%, #253174 100%);
    transition: transform 0.2s;
  }
}

.dashboardBtns {
    margin-top: 0.5rem;
    padding: 0.5rem 1.5rem;
    border-radius: 0.375rem;
    color: white;
    cursor: pointer;
    background: linear-gradient(135deg, #1e3ec8 0%, #253174 100%);
    transition: transform 0.2s;
}

.tableClick {
  display: grid;
  align-items: center;
  justify-content: center;
  max-height: 400px; /* Adjust as needed */
  overflow-y: auto;
  width: 100%;
  /* table {
    background-color: red;
  } */
}

.create-url {
  input {
    width: 100%;
    padding: 0.75rem;
    border: 1px solid #2c3e50;
    border-radius: 5px;
    font-size: 1rem;
    transition: border-color 0.3s;
    box-sizing: border-box;
  }
}

.dashboardBtns:hover, .urlCreation button:hover, .viewWallet button:hover, .logoutBtn:hover {
    transform: scale(1.05);
}

.urlResponse {
  display: grid;
  align-items: center;
  justify-content: center;
  margin-top: 1rem;
  padding: 0.5rem 1.5rem;
}

.newUrlCreated {
  margin-top: 2%;
  color: #3182ce; /* Blue-600 */
  text-decoration: underline;
}

</style>
