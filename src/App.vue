<script setup>
import { ref } from 'vue';

const newTodo = ref('');

const todosData = JSON.parse(localStorage.getItem('todos')) || []

const todos = ref(todosData)
const addTodo = () => {
  if(newTodo.value) {
    todos.value.push({
      text: newTodo.value,
      done: false
    })

    newTodo.value = ''

    saveData()
  }
}

const doneTodo = (todo) => {
  todo.done = !todo.done
  saveData()
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
  saveData()
}

const saveData = () => {
  localStorage.setItem('todos', JSON.stringify(todos.value))
}

</script>

<template>
  <h1>Todo</h1> 

  <form 
    @submit.prevent="addTodo"
    @keypress.enter="addTodo"
  >
      <label for="">
        New Todo
      </label>

      <input 
        v-model="newTodo"
        type="text"
        >

      <button type="submit">Add</button>
  </form>

  <h2>Todo List</h2>

  <ul>
    <li v-for="(todo, index) in todos" :key="index">
      <span 
        :class="{ done: todo.done }"
        @click="doneTodo(todo)"
      >
        {{ todo.text }}
      </span>

      <button @click="removeTodo(index)">Delete</button>
    </li>
  </ul>

  <h4 v-if="!todos.length">Empty Todo List</h4>
</template>

<style scoped>

</style>
