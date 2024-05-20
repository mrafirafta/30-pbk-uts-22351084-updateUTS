<template>
    <div>
      <slot name="header"></slot>
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
            <td><button @click="removeActivity(index)">Hapus</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  // Define props
  const props = defineProps({
    initialTodos: {
      type: Array,
      default: () => []
    }
  });
  
  // Local state
  const newActivity = ref({
    name: '',
    dateTime: ''
  });
  
  const todos = ref([...props.initialTodos]);
  
  // Methods
  const addActivity = () => {
    if (newActivity.value.name.trim() !== '' && newActivity.value.dateTime.trim() !== '') {
      todos.value.push({
        name: newActivity.value.name,
        dateTime: newActivity.value.dateTime,
        completed: false
      });
      newActivity.value.name = '';
      newActivity.value.dateTime = '';
    }
  };
  
  const removeActivity = (index) => {
    todos.value.splice(index, 1);
  };
  
  const formatDate = (dateTime) => {
    const options = { year: 'numeric', month: 'short', day: 'numeric', hour: 'numeric', minute: 'numeric' };
    return new Date(dateTime).toLocaleDateString('en-US', options);
  };
  </script>
  
  <style scoped>
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
  </style>
  