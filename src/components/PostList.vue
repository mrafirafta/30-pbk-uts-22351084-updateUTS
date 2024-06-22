<template>
  <section class="post-section">
    <h2>Postingan Pengguna</h2>
    <div class="select-user">
      <label for="selectUser">Pilih Pengguna:</label>
      <select v-model="selectedUser" @change="fetchPosts" id="selectUser" class="select-box">
        <option disabled value="">Pilih pengguna</option>
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
    </div>
    <div class="search-box">
      <label for="searchInput">Cari Postingan:</label>
      <input type="text" v-model="searchKeyword" @input="searchPosts" id="searchInput" placeholder="Masukkan kata kunci...">
    </div>
    <div v-if="posts.length > 0" class="user-posts">
      <h3 class="user-posts-heading">Postingan Pengguna: {{ selectedUserName }}</h3>
      <div class="user-details">
        <p><strong>Nama:</strong> {{ selectedUserName }}</p>
        <p><strong>Email:</strong> {{ selectedUserEmail }}</p>
      </div>
      <div class="posts-container">
        <div v-for="post in filteredPosts" :key="post.id" class="post-card" @click="viewPostDetails(post.id)">
          <div class="post-card-content">
            <h4 class="post-title">{{ post.title }}</h4>
            <p class="post-body">{{ post.body }}</p>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="no-posts">
      <p>Tidak ada postingan yang tersedia.</p>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'
import { useRouter } from 'vue-router'

const selectedUser = ref(null)
const users = ref([])
const posts = ref([])
const searchKeyword = ref('')
const selectedUserName = ref('')
const selectedUserEmail = computed(() => {
  const selectedUserObj = users.value.find(user => user.id === selectedUser.value)
  return selectedUserObj ? selectedUserObj.email : ''
})

const router = useRouter()

const fetchUsers = async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')
    users.value = await response.json()
  } catch (error) {
    console.error('Error fetching users:', error)
  }
}

const fetchPosts = async () => {
  if (!selectedUser.value) return
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`)
    posts.value = await response.json()
    selectedUserName.value = users.value.find(user => user.id === selectedUser.value)?.name || ''
  } catch (error) {
    console.error('Error fetching posts:', error)
  }
}

const searchPosts = () => {
  // Implement search functionality based on 'searchKeyword'
}

const viewPostDetails = (postId) => {
  router.push(`/posts/${postId}`)
}

const filteredPosts = computed(() => {
  // Filter posts based on 'searchKeyword'
  if (!searchKeyword.value.trim()) {
    return posts.value
  } else {
    const keyword = searchKeyword.value.trim().toLowerCase()
    return posts.value.filter(post => post.title.toLowerCase().includes(keyword) || post.body.toLowerCase().includes(keyword))
  }
})

onMounted(fetchUsers)
</script>

<style scoped>
.post-section {
  margin-top: 20px;
  padding: 20px;
  background-color: #ffffff; /* Warna latar belakang abu-abu */
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  max-width: 800px;
  margin: 0 auto;
}

.select-user {
  margin-bottom: 20px;
}

.select-box {
  padding: 8px 12px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #fff;
  transition: border-color 0.3s, background-color 0.3s;
  width: 100%;
  max-width: 300px;
}

.select-box:focus {
  outline: none;
  border-color: #007bff;
  background-color: #fff;
}

.search-box {
  margin-bottom: 20px;
}

.search-box label {
  margin-right: 10px;
  font-size: 16px;
  color: #333;
}

.search-box input {
  padding: 8px 12px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 100%;
  max-width: 400px;
  transition: border-color 0.3s;
}

.search-box input:focus {
  outline: none;
  border-color: #007bff;
}

.user-posts {
  background-color: #fff; /* Latar belakang putih */
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.user-posts-heading {
  font-size: 24px;
  color: #333; /* Warna teks abu-abu gelap */
  margin-bottom: 15px;
}

.user-details {
  margin-bottom: 20px;
}

.posts-container {
  display: grid;
  gap: 20px;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
}

.post-card {
  cursor: pointer;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, box-shadow 0.2s;
}

.post-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}

.post-card-content {
  padding: 15px;
}

.post-title {
  font-size: 18px;
  color: #333;
  margin-bottom: 10px;
}

.post-body {
  font-size: 14px;
  color: #555;
}

.no-posts {
  text-align: center;
  margin-top: 20px;
}

.no-posts p {
  color: #777;
}

.loading-indicator {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 200px;
}

.loading-indicator p {
  font-size: 18px;
  color: #555;
}
</style>
