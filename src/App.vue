<template>
  <h1>TODO LIST</h1>
  <form action="" @submit.prevent="addTodo">

  <fieldset role="group">
    <input
      v-model="newTodo"
      id="new"
      type="text" 
      placeholder="Nouvelle tâche">
    <button :disabled="newTodo.lenght === 0">Ajouter la tâche</button>
  </fieldset>
  </form>
  <div v-if="todos.lenght === 0">Vous n'avez pas de tâches à effectuer.</div>
  <div v-else>
    <ul>
      <li 
      v-for="todo in sortedTodos()" 
      :key="todo.date"
      :class="{completed: todo.completed}"
      > 
        <label>
          <input type="checkbox" v-model="todo.completed">
          {{ todo.title }}
        </label>
      </li>
    </ul>
    <label>
      <input type="checkbox" v-model="hideCompleted">
      Masquer les tâches complétées
    </label>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const todos = ref([{
  title: 'Tâche de test',
  completed: true,
  date: 1,
},{
  title: 'Tâche à faire',
  completed: false,
  date: 2,
}])
const newTodo = ref('')
const hideCompleted = ref(false)
const addTodo = () => {
  todos.value.push({
    title: newTodo.value,
    completed: false,
    date: Date.now()
  })
  newTodo.value = ''
}

const sortedTodos = () => {
  const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
  if (hideCompleted.value === true) {
    return sortedTodos.filter(t => t.completed === false)
  }
  return sortedTodos
}

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

button {
  display: flex;
  align-items: center;
  font-size: small;
}

.completed {
  opacity: .5;
  text-decoration: line-through;
}
</style>