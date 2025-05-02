<template>
  <div class="container">
    <h1>To Do List</h1>

    <!-- Form Tambah Kegiatan -->
    <form @submit.prevent="addTodo" class="form">
      <input
        v-model="newTodo"
        placeholder="Masukkan kegiatan..."
        class="todo-input"
      />
      <button type="submit" class="add-btn">Tambah</button>
    </form>

    <!-- Dropdown Filter -->
    <div class="filter-container">
      <select id="filter-select" v-model="filter">
        <option value="all">Semua Kegiatan</option>
        <option value="pending">Belum Selesai</option>
        <option value="completed">Sudah Selesai</option>
      </select>
    </div>

    <!-- Daftar Kegiatan -->
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

    <!-- Footer -->
    <footer class="footer">
      <p>&copy; <span class="name">Adinda Meilani Putri</span> - All Rights Reserved</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const filter = ref('all')

const todos = ref([
  { text: 'makan', completed: false },
  { text: 'minum', completed: false },
  { text: 'sholat', completed: true }
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
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600&display=swap');

.container {
  max-width: 600px;
  margin: 40px auto;
  padding: 24px;
  border-radius: 20px;
  background: linear-gradient(135deg, #1f1c2c, #928dab);
  color: #ffffff;
  font-family: 'Orbitron', sans-serif;
  box-shadow: 0 8px 32px rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.18);
  transition: box-shadow 0.3s ease-in-out;
}

.container:active {
  box-shadow: 0 10px 40px rgba(31, 38, 135, 0.5);
}

h1 {
  text-align: center;
  font-size: 28px;
  margin-bottom: 20px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
}

/* Form */
.form {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
  align-items: center;
}

.todo-input {
  flex: 1;
  padding: 14px 20px;
  font-size: 16px;
  border-radius: 10px;
  border: 2px solid #ffffff80;
  background: rgba(255, 255, 255, 0.12);
  color: #ffffff;
  height: 50px;
  box-sizing: border-box;
  transition: border 0.3s ease, box-shadow 0.3s ease;
}

.todo-input::placeholder {
  color: #dddddd;
  font-size: 14px;
}

.todo-input:focus {
  border-color: #00c9a7;
  outline: none;
  box-shadow: 0 0 8px rgba(0, 201, 167, 0.8);
}

.add-btn {
  background: #ffffff;
  color: #1f1c2c;
  font-weight: bold;
  font-size: 16px;
  border: 2px solid #1f1c2c;
  padding: 0 20px;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.add-btn:hover {
  background: #00c9a7;
  color: white;
  border-color: #00a388;
}

.add-btn:active {
  transform: translateY(2px);
  box-shadow: 0 4px 6px rgba(0, 201, 167, 0.5);
}

/* Filter */
.filter-container {
  display: flex;
  justify-content: flex-start;
  margin-bottom: 20px;
  padding-left: 4px;
}

select {
  padding: 8px 12px;
  font-size: 16px;
  border-radius: 8px;
  border: none;
  background: rgba(255, 255, 255, 0.15);
  color: white;
  backdrop-filter: blur(5px);
  cursor: pointer;
  appearance: none;
  background-image: url("data:image/svg+xml;charset=US-ASCII,%3Csvg%20fill%3D'white'%20height%3D'24'%20viewBox%3D'0%200%2024%2024'%20width%3D'24'%20xmlns%3D'http%3A//www.w3.org/2000/svg'%3E%3Cpath%20d%3D'M7%2010l5%205%205-5z'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-size: 16px 16px;
  transition: transform 0.2s ease;
}

select:focus {
  transform: translateY(-2px);
}

select option {
  background: #2c2c54;
  color: white;
}

/* To-Do List */
ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  padding: 12px;
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.2);
  margin-bottom: 12px;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  transition: transform 0.2s ease, box-shadow 0.3s ease;
}

li:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 14px rgba(0, 0, 0, 0.3);
}

input[type="checkbox"] {
  transform: scale(1.3);
  accent-color: #00c9a7;
}

li.done span {
  text-decoration: line-through;
  color: #bbb;
  opacity: 0.7;
}

li span {
  flex: 1;
  margin-left: 12px;
  font-size: 18px;
}

.delete {
  background: transparent;
  color: #ff6b6b;
  font-size: 22px;
  border: none;
  cursor: pointer;
  transition: transform 0.2s ease, color 0.3s ease;
}

.delete:hover {
  transform: scale(1.2);
  color: #ff4d4d;
}

.delete:active {
  transform: translateY(2px);
}

/* Footer */
.footer {
  text-align: center;
  font-size: 14px;
  color: #ffffff;
  margin-top: 40px;
  animation: slideIn 3s ease-in-out infinite alternate;
}

.footer p {
  margin: 0;
  font-family: 'Orbitron', sans-serif;
}

.footer .name {
  color: #00c9a7;
  font-weight: bold;
  font-size: 18px;
}

@keyframes slideIn {
  0% {
    transform: translateY(10px);
    opacity: 0;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}

@media (max-width: 600px) {
  .container {
    padding: 16px;
    margin: 20px auto;
  }

  h1 {
    font-size: 22px;
  }

  .form {
    flex-direction: column;
    gap: 10px;
  }

  .todo-input,
  .add-btn {
    width: 100%;
    height: 45px;
    font-size: 15px;
  }

  .filter-container {
    justify-content: center;
  }

  select {
    width: 100%;
    font-size: 15px;
  }

  li span {
    font-size: 16px;
  }

  .footer {
    font-size: 12px;
  }

  .footer .name {
    font-size: 16px;
  }
}



</style>