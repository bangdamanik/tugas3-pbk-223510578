<template>
  <div class="todo-app">
    <form @submit.prevent="addTodo" class="todo-form">
      <input v-model="newTodo" required placeholder="Add a new task..." class="todo-input">
      <button type="submit" class="todo-button">Add Todo</button>
    </form>

    <ul class="todo-list">
      <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
        <input type="checkbox" v-model="todo.done">
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button @click="removeTodo(todo)" class="delete-button">X</button>
      </li>
    </ul>

    <button @click="hideCompleted = !hideCompleted" class="toggle-completed">
      {{ hideCompleted ? 'Show all' : 'Hide completed' }}
    </button>
    <p class="message">{{ pesan }}</p>
  </div>
</template>

<script setup>
import { ref, computed, onUpdated, watch } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'Belajar', done: true },
  { id: id++, text: 'Cuci Mobil', done: true },
  { id: id++, text: 'Beli Makanan', done: false }
])

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t.id !== todo.id)
}

onUpdated(() => {
  console.log("A change occurred")
})

const pesan = ref("")
watch(newTodo, (newValue) => {
  if (newValue.includes("?")) {
    pesan.value = "No question marks allowed"
  } else {
    pesan.value = ""
  }
})
</script>

<style scoped>
.todo-app {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f7f7f7;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.todo-form {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

.todo-button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.todo-button:hover {
  background-color: #0056b3;
}

.todo-list {
  list-style-type: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.todo-item:last-child {
  border-bottom: none;
}

.done {
  text-decoration: line-through;
  color: #888;
}

.delete-button {
  background-color: transparent;
  border: none;
  color: #e74c3c;
  cursor: pointer;
}

.delete-button:hover {
  color: #c0392b;
}

.toggle-completed {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.toggle-completed:hover {
  background-color: #218838;
}

.message {
  margin-top: 20px;
  color: #e74c3c;
}
</style>