<template>
  <div class="container">
    <h1> To Do List</h1>

    <form @submit.prevent="addTodo" class="form">
      <input v-model="newTodo" placeholder="Masukkan kegiatan..." />
      <button type="submit">Tambah</button>
    </form>

    <!-- Dropdown Filter -->
    <div class="dropdown-filter">
      <label for="filter-select">Filter:</label>
      <select id="filter-select" v-model="filter">
        <option value="all">Semua Kegiatan</option>
        <option value="pending"> Belum Selesai</option>
        <option value="completed"> Sudah Selesai</option>
      </select>
    </div>

    <ul>
      <li
        v-for="(todo, index) in filteredTodos"
        :key="'todo-' + index"
        :class="{ done: todo.completed }"
      >
        <input type="checkbox" v-model="todo.completed" />
        <span>{{ todo.text }}</span>
        <button class="delete" @click="removeTodo(index)">❌</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const filter = ref('all')

const todos = ref([
  { text: 'makan', completed: false },
  { text: 'tidur', completed: true }
])

const addTodo = () => {
  if (newTodo.value.trim() === '') return
  todos.value.push({ text: newTodo.value.trim(), completed: false })
  newTodo.value = ''
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() => {
  if (filter.value === 'pending') {
    return todos.value.filter(todo => !todo.completed)
  } else if (filter.value === 'completed') {
    return todos.value.filter(todo => todo.completed)
  } else {
    return todos.value
  }
})
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 40px auto;
  background: #f0f4f8;
  border-radius: 16px;
  padding: 24px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  font-family: 'Segoe UI', sans-serif;
}

h1 {
  text-align: center;
  color: #2c3e50;
}

.form {
  display: flex;
  gap: 10px;
  margin-bottom: 16px;
}

input[type="text"],
input[type="checkbox"] {
  padding: 8px;
  font-size: 16px;
}

input[type="text"] {
  flex: 1;
  border-radius: 6px;
  border: 1px solid #ccc;
}

button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 8px 14px;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  background-color: #369c77;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  padding: 10px;
  background: white;
  margin-bottom: 10px;
  border-radius: 8px;
}

li.done span {
  text-decoration: line-through;
  color: gray;
}

li span {
  flex: 1;
  margin-left: 10px;
  font-size: 16px;
}

.delete {
  background: transparent;
  color: red;
  font-size: 18px;
  border: none;
  cursor: pointer;
}

.dropdown-filter {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 16px;
  gap: 10px;
  font-size: 16px;
  color: #2c3e50;
}

select {
  padding: 6px 12px;
  font-size: 16px;
  border-radius: 6px;
  border: 1px solid #ccc;
  cursor: pointer;
}
</style>