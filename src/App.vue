<template>
<div id="app">
    <h1>ToDo App</h1>
    <form :style="{'text-align':'center'}" @submit.prevent="addTodo()">
        <label :style="{'text-align':'center'}" >New Tasks </label>
        <input :style="{'text-align':'center'}" v-model="newTodo" name="newTodo" autocomplete="off">
        <button>Add Tasks</button>
    </form>
    <h2>ToDo List</h2>
    <ul :style="{'text-align':'center'}">
      <li
        v-for="(todo, index) in todos"
        :key="index"
      >
        <span
          :class="{ done: todo.done }"
          @click="doneTodo(todo)"
        >{{ todo.content }}</span>
        <button class="button" @click="removeTodo">Remove</button>
      </li>
    </ul>
    <h4 :style="{'text-align':'center'}" v-if="todos.length === 0">Empty list.</h4>
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
@import url('https://fonts.googleapis.com/css?family=Fjalla+One'); 

body {
  background-color: #ffab57;
  font-family: 'Fjalla One', sans-serif;
  text-transform: uppercase;
  height: 95vh;
  display: flex;
  justify-content: center;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
}

button {
	border-radius: 4px;
	background-color: #9000ff;
	border: none;
	color: #fff;
	text-align: center;
	font-size: 32px;
	padding: 16px;
	width: 170px;
	transition: all 0.5s;
	cursor: pointer;
	margin: 36px;
	box-shadow: 0 10px 20px -8px rgba(0, 0, 0,.7);
}
  
.button{
cursor: pointer;
display: inline-block;
position: relative;
transition: 0.5s;
 }
 
.button:after {
content: '»';
position: absolute;
opacity: 0;  
top: 14px;
right: -20px;
transition: 0.5s;
 }
 
.button:hover{
padding-right: 24px;
padding-left:8px;
 }
 
.button:hover:after {
opacity: 1;
right: 10px;
 }

#app {
max-width: 600px;
margin-left: auto;
margin-right: auto;
padding: 20px;
}
h1 {
  font-weight: bold;
  font-size: 28px;
  text-align: center;
}
h2 {
  font-weight: bold;
  font-size: 24px;
  text-align: center;
  margin-top: 20px;
}

.done {
  text-decoration: line-through;
}
</style>