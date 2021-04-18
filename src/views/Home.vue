<template>
  <div id="app">
    <add-todo v-on:add-Todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>
<script>
import Todos from '../components/Todos';
import AddTodo from "../components/AddTodo";
import axios from 'axios';
export default {
  name: 'Home',
  components: {
    AddTodo,
    Todos,
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
          .then(res => this.todos.splice(res.data, 1))
          .catch(err => console.log(err))
    },
    addTodo(newTodo) {
      const {title, completed} = newTodo;
      axios.post('https://jsonplaceholder.typicode.com/todos', {
        title,
        completed
      })
          .then(res => this.todos = [...this.todos, res.data])
          .catch(err => console.log(err))
    }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=10')
        .then(res => this.todos = res.data)
        .catch(err => console.log(err))
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, SansSerif;
  line-height: 1.4;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
</style>
