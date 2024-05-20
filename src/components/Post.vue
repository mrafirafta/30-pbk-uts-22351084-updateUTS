<template>
    <div>
      <slot name="header"></slot>
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
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  // Define props
  const props = defineProps({
    initialPosts: {
      type: Array,
      default: () => []
    }
  });
  
  // Local state
  const users = ref([]);
  const selectedUser = ref(null);
  const newPost = ref({
    userId: null,
    title: '',
    body: ''
  });
  const posts = ref([...props.initialPosts]);
  
  // Methods
  const fetchUsers = async () => {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/users');
      users.value = await response.json();
    } catch (error) {
      console.error('Error fetching users:', error);
    }
  };
  
  const fetchUserPosts = async () => {
    if (selectedUser.value) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`);
        posts.value = await response.json();
      } catch (error) {
        console.error('Error fetching user posts:', error);
      }
    }
  };
  
  const createPost = async () => {
    if (newPost.value.userId && newPost.value.title.trim() !== '' && newPost.value.body.trim() !== '') {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(newPost.value)
        });
        const postData = await response.json();
        posts.value.unshift(postData);
        newPost.value.title = '';
        newPost.value.body = '';
      } catch (error) {
        console.error('Error creating post:', error);
      }
    }
  };
  
  onMounted(fetchUsers);
  </script>
  
  <style scoped>
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
  
  .post-list .post {
    border: 1px solid #ddd;
    padding: 10px;
    margin-bottom: 10px;
  }
  
  .post-list .post h3 {
    margin: 0 0 10px;
  }
  
  .post-list .post p {
    margin: 0;
  }
  </style>
  