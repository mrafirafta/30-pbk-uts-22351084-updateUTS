<template>
  <div class="container">
    <h1>What's your plan today?</h1>
    <div class="header-menu">
      <button @click="selectedMenu = 'Todos'" :class="{ 'active': selectedMenu === 'Todos' }">Todos</button>
      <button @click="selectedMenu = 'Post'" :class="{ 'active': selectedMenu === 'Post' }">Post</button>
    </div>
    <Todos v-if="selectedMenu === 'Todos'">
      <template #header>
        <h2>Your Todos</h2>
      </template>
    </Todos>
    <Post v-else-if="selectedMenu === 'Post'">
      <template #header>
        <h2>User Posts</h2>
      </template>
    </Post>
  </div>
</template>

<script>
import Todos from './components/Todos.vue';
import Post from './components/Post.vue';

export default {
  components: {
    Todos,
    Post
  },
  data() {
    return {
      selectedMenu: 'Todos' // Default menu selected
    };
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
</style>
