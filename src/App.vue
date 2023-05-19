<template>
<div id="app">
    <h1>ToDo App</h1>
    <form @submit.prevent="addTodo()">
        <label>New ToDo </label>
        <input v-model="newTodo" name="newTodo" autocomplete="off">
        <button>Add ToDo</button>
    </form>
    <h2>ToDo List</h2>
    <ul>
      <li
        v-for="(todo, index) in todos"
        :key="index"
      >
        <span
          :class="{ done: todo.done }"
          @click="doneTodo(todo)"
        >{{ todo.content }}</span>
        <button @click="removeTodo(index)">Remove</button>
      </li>
    </ul>
    <h4 v-if="todos.length === 0">Empty list.</h4>
</div>
</template>

<script>
import { ref } from 'vue'
export default {
  name: 'App',
  setup () {
    const newTodo = ref('')
    const defaultData = [{
      done: false,
      content: 'write a line'
    }]
    const TodoData = JSON.parse(localStorage.getItem('todos')) || defaultData
    const todos = ref(TodoData)
    function addTodo () {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value
        })
        newTodo.value = ''
      }
      saveData()
    }

    function doneTodo (Todo) {
      Todo.done = !Todo.done
      saveData()
    }

    function removeTodo (index) {
      todos.value.splice(index, 1)
      saveData()
    }

    function saveData () {
      const parsed = JSON.stringify(todos.value)
      localStorage.setItem('todos', parsed)
    }

    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData
    }
  }
}
</script>

<style>
.done {
  text-decoration: line-through;
}
</style>// eslint-disable-next-line