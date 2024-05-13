<!-- src/App.vue -->
<!-- <template>
  <div id="app" :style="{ backgroundColor: '#fff' }">
    <ActivityList />
  </div>

</template>

<script>
import ActivityList from './components/ActivityList.vue';

export default {
  components: {
    ActivityList
  }
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  margin: 0 auto;
  padding: 20px;
  max-width: 600px;
}
</style> -->

<template>
  <div class="container">
    <h1>What's your plan today?</h1>
    <div class="header-menu">
      <button @click="selectedMenu = 'Todos'" :class="{ 'active': selectedMenu === 'Todos' }">Todos</button>
      <button @click="selectedMenu = 'Post'" :class="{ 'active': selectedMenu === 'Post' }">Post</button>
    </div>
    <div v-if="selectedMenu === 'Todos'">
      <h2>Your Todos</h2>
      <div class="input-container">
        <input type="text" v-model="newActivity.name" placeholder="Tambahkan kegiatan baru">
        <input id="datetime" type="datetime-local" v-model="newActivity.dateTime">
        <button @click="addActivity">Tambah</button>
      </div>
      <table>
        <thead>
          <tr>
            <th>Kegiatan</th>
            <th>Tanggal & Jam</th>
            <th>Status</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(activity, index) in todos" :key="index" :class="{ 'completed': activity.completed }">
            <td>{{ activity.name }}</td>
            <td>{{ formatDate(activity.dateTime) }}</td>
            <td>
              <input type="checkbox" v-model="activity.completed">
              <span>{{ activity.completed ? 'Selesai' : 'Belum Selesai' }}</span>
            </td>
            <td><button @click="removeActivity(index, 'todos')">Hapus</button></td>
          </tr>
        </tbody>
      </table>
    </div>
    <div v-else-if="selectedMenu === 'Post'">
      <h2>User Posts</h2>
      <div class="post-form">
        <select v-model="selectedUser" @change="fetchUserPosts">
          <option disabled value="">Pilih Pengguna</option>
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
        <button @click="createPost">Post</button>
      </div>
      <div class="post-list">
        <div v-for="post in posts" :key="post.id" class="post">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newActivity: {
        name: '',
        dateTime: ''
      },
      todos: [],
      selectedMenu: 'Todos', // Default menu selected
      users: [],
      selectedUser: null,
      newPost: {
        userId: null,
        title: '',
        body: ''
      },
      posts: []
    };
  },
  methods: {
    addActivity() {
      if (this.newActivity.name.trim() !== '' && this.newActivity.dateTime.trim() !== '') {
        this.todos.push({ 
          name: this.newActivity.name, 
          dateTime: this.newActivity.dateTime, 
          completed: false 
        });
        this.newActivity.name = '';
        this.newActivity.dateTime = '';
      }
    },
    removeActivity(index, listName) {
      if (listName === 'todos') {
        this.todos.splice(index, 1);
      }
    },
    formatDate(dateTime) {
      const options = { year: 'numeric', month: 'short', day: 'numeric', hour: 'numeric', minute: 'numeric' };
      return new Date(dateTime).toLocaleDateString('en-US', options);
    },
    async fetchUsers() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        this.users = await response.json();
      } catch (error) {
        console.error('Error fetching users:', error);
      }
    },
    async fetchUserPosts() {
      if (this.selectedUser) {
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
          this.posts = await response.json();
        } catch (error) {
          console.error('Error fetching user posts:', error);
        }
      }
    },
    async createPost() {
      if (this.newPost.userId && this.newPost.title.trim() !== '' && this.newPost.body.trim() !== '') {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(this.newPost)
          });
          const postData = await response.json();
          this.posts.unshift(postData);
          this.newPost.title = '';
          this.newPost.body = '';
        } catch (error) {
          console.error('Error creating post:', error);
        }
      }
    }
  },
  async mounted() {
    await this.fetchUsers();
  }
};
</script>

<style scoped>
/* Styling for active menu */
.active {
  background-color: #45a049;
}

/* Other styles remain unchanged */
.container {
  max-width: 600px;
  margin: 0 auto;
}

h1 {
  text-align: center;
}

.header-menu {
  text-align: center;
  margin-bottom: 20px;
}

.input-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 8px;
  margin-right: 10px;
}

button {
  padding: 8px 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border-bottom: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #4CAF50;
  color: white;
}

tr:hover {
  background-color: #f5f5f5;
}

.completed {
  text-decoration: line-through;
}

.post-form {
  margin-bottom: 20px;
}

.post-form select {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
}

.post-form textarea {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  resize: vertical;
}

.post-form button {
  width: 100%;
  padding: 8px 16px;
}
</style>



