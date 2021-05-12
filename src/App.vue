<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Header from './components/Layout/Header'
import Todos from './components/Todos';
import AddTodo from './components/AddTodo';
import axios from 'axios';
import uuid from 'uuid';


export default {
  name: 'App',
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos${id}`)
      .then(this.todos = this.todos.filter(todo => todo.id !== id))
      .catch(err => console.log(err));
      console.log("deleted")
    },
    addTodo(NewTodo) {
      const {title, completed} = NewTodo;
      var ID = uuid.v4() //If id is not defined in database

      axios.post('https://jsonplaceholder.typicode.com/todos', {
        completed,
        title,
      })
      .then(res => 
      {
        if(res.data.id == 201)
        {
          this.todos = [...this.todos, {id: ID, title: title, completed: completed}]
          console.log(this.todos)
        }
        else
        {
          this.todos = [...this.todos, res.data]
          console.log(this.todos)
        }
      })
      .catch(err => console.log(err));
    }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=0')
    .then(res => this.todos = res.data)
    .catch(err => console.log(err));
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
    font-family: Arial, Helvetica, sans-serif;
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
  .btn:hover {
    background: #666;
  }
</style>
