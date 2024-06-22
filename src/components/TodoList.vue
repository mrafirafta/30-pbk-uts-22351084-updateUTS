<template>
  <div class="body">
    <section class="create-todo">
      <div class="container">
        <h3 style="color: black;">APA YANG KAMU INGIN LAKUKAN HARI INI</h3>
        <form @submit.prevent="addTodo">
          <input type="text" placeholder="Masukkan kegiatan" v-model="inputContent" />
          <h4>Pilih kategori:</h4>
          <div class="options">
            <label>
              <input type="radio" name="category" value="home" v-model="inputCategory" />
              <span class="bubble home">Rumah</span>
              <div></div>
            </label>
            <label>
              <input type="radio" name="category" value="assignment" v-model="inputCategory" />
              <span class="bubble task">Tugas</span>
              <div></div>
            </label>
            <label>
              <input type="radio" name="category" value="work" v-model="inputCategory" />
              <span class="bubble work">Kerja</span>
              <div></div>
            </label>
          </div>
          <button type="submit">Submit</button>
        </form>
        <div class="todo-list">
          <table>
            <thead>
              <tr>
                <th>Kegiatan</th>
                <th>Kategori</th>
                <th>Aksi</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="todo in todosAsc" :key="todo.createdAt" :class="`todo-item ${todo.done ? 'done' : ''}`">
                <td>{{ todo.content }}</td>
                <td>{{ todo.category }}</td>
                <td>
                  <button class="delete" @click="removeTodo(todo)">Delete</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const todos = ref([])
const inputContent = ref('')
const inputCategory = ref(null)

const todosAsc = computed(() => todos.value.slice().sort((a, b) => b.createdAt - a.createdAt))

const addTodo = () => {
  if (inputContent.value.trim() === '' || inputCategory.value === null) {
    return
  }

  todos.value.push({
    content: inputContent.value,
    category: inputCategory.value,
    done: false,
    createdAt: new Date().getTime()
  })

  inputContent.value = ''
  inputCategory.value = null
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<style>
.create-todo h3 {
  font-size: 30px;
  font-weight: bold;
  font-family: 'Oswald', sans-serif;
}

.create-todo, .todo-list {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
  padding: 20px;
  font-family: 'Oswald', sans-serif;
}

.container {
  background-color: white;
  border-radius: 20px;
  padding: 20px;
}

.create-todo form {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgb(255, 251, 251);
  margin-bottom: 20px;
}

.create-todo .options {
  display: flex;
  justify-content: center;
  margin: 10px 0;
}

.create-todo .options label {
  margin: 0 10px;
  display: flex;
  align-items: center;
  cursor: pointer;
}

.create-todo .options input[type="radio"] {
  display: none;
}

.create-todo .options .bubble {
  width: 50px;
  height: 30px;
  border-radius: 2px;
  margin-right: 1px;
  border: 2px solid transparent;
  transition: all 0.3s;
  margin-bottom: 10px;
  margin-top: 10px;
}

.create-todo .options input[type="radio"]:checked + .bubble {
  border-color: #000000;
}

.create-todo .options .home { background: #e2e2e2; }
.create-todo .options .task { background: #e2e2e2; }
.create-todo .options .work { background: #e2e2e2; }

.create-todo .options .home:hover { background: #ff5050;color:white;transform: scale(1.1); }
.create-todo .options .task:hover { background: #ff5050;color:white;transform: scale(1.1); }
.create-todo .options .work:hover { background: #ff5050;color:white;transform: scale(1.1); }

button[type="submit"] {
  padding: 10px 20px;
  border: none;
  background: #00ff51;
  color: white;
  cursor: pointer;
  border-radius: 5px;
  transition: background 0.3s, transform 0.2s;
  font-family: 'Oswald', sans-serif;
}

button[type="submit"]:hover {
  background: #0056b3;
  transform: scale(1.05);
}

.todo-list table {
  width: 100%;
  border-collapse: collapse;
  font-family: 'Oswald', sans-serif;
  background-color: #fff;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.todo-list th, .todo-list td {
  padding: 10px;
  border-bottom: 1px solid #ddd;
  color: #333;
}

.todo-list th {
  background-color: #f2f2f2;
}

.todo-list tr:hover {
  background-color: #f5f5f5;
}

.todo-list .todo-item.done {
  background: #e0ffe0;
}

.todo-list .todo-item .actions .delete {
  color: #dc3545;
  cursor: pointer;
  transition: color 0.3s;
}

.todo-list .todo-item .actions .delete:hover {
  color: #c82333;
}

.body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-image: url('https://i.ytimg.com/vi/t4akCDyUz-0/maxresdefault.jpg');
  background-size: cover;
  background-position: center;
  position: relative;
}

.body::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: -1;
}
</style>
