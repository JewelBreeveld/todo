<template>
  <div id="app">
    <Header/>
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from "./components/Todos";
import Header from "./components/Layout/Header";
import AddTodo from "./components/AddTodo";
import fetch from "node-fetch";

export default {
  name: "app",
  components: {
    Todos,
    Header,
    AddTodo
  },

  data() {
    return {
      todos: []
    };
  },
  methods: {
    deleteTodo(id) {
      fetch(`https://jsonplaceholder.typicode.com/todos/${id}`, {
        method: "delete",
        body: id,
        headers: { "Content-Type": "text/html" }
      })
        .then(res => (this.todos = this.todos.filter(todo => todo.id !== id)))
        .catch(err => console.log(err));
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;
      fetch("https://jsonplaceholder.typicode.com/todos", {
        method: "post",
        body: JSON.stringify(newTodo),
        headers: { "Content-Type": "application/json" }
      })
        .then(res => res.json())
        .then(data => (this.todos = [...this.todos, data]))
        .catch(err => console.log(err));
    }
  },
  created() {
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=10")
      .then(res => res.json())
      .then(data => {
        this.todos = data;
        console.log("todos", this.todos);
      })
      .catch(err => err);
  }
};
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
