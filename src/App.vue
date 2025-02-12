<template>
  <h1>TODO LIST</h1>
  <form action="" @submit.prevent="addTodo">

  <fieldset role="group">
    <input
      v-model="newTodo"
      id="new"
      type="text" 
      placeholder="Nouvelle tâche">
    <button :disabled="newTodo.length === 0">Ajouter la tâche</button>
  </fieldset>
  </form>
  <div v-if="todos.length === 0">Vous n'avez pas de tâches à effectuer.</div>
  <div v-else>
    <ul>
      <li 
      v-for="todo in sortedTodos()" 
      :key="todo.date"
      :class="{completed: todo.completed}"
      > 
        <label>
          <input type="checkbox" v-model="todo.completed" @change="saveTodos">
          {{ todo.title }}
        </label>
        <button class="delete-button" @click="deleteTodo(todo)">❌</button>
      </li>
    </ul>
    <label>
      <input type="checkbox" v-model="hideCompleted">
      Masquer les tâches complétées
    </label>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';

const todos = ref(JSON.parse(localStorage.getItem('todos')) || [{
  title: 'Tâche de test',
  completed: true,
  date: 1,
},{
  title: 'Tâche de test 2',
  completed: false,
  date: 2,
}])
const newTodo = ref('')
const hideCompleted = ref(false)

const saveTodos = () => {
  localStorage.setItem('todos', JSON.stringify(todos.value))
}

const addTodo = () => {
  todos.value.push({
    title: newTodo.value,
    completed: false,
    date: Date.now()
  })
  newTodo.value = ''
  saveTodos()
}

const deleteTodo = (todo) => {
  todos.value = todos.value.filter(t => t !== todo)
  saveTodos()
}

const sortedTodos = () => {
  const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
  if (hideCompleted.value === true) {
    return sortedTodos.filter(t => t.completed === false)
  }
  return sortedTodos
}

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || todos.value
})

watch(todos, saveTodos, { deep: true })

</script>

<style>

h1 {
  display: flex;
  justify-content: center;
  margin: 2rem;
}

fieldset {
  height: 3.125rem;
}

li {
  display: flex;
  justify-content: space-between;
}

button {
  display: flex;
  align-items: center;
  font-size: small;
}

.delete-button {
  background-color: #13171F;
  border: none;
  padding: 0;
  margin-left: 10px;
  color: white;
  cursor: pointer;
}

.completed {
  opacity: .5;
  text-decoration: line-through;
}
</style>