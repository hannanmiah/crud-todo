<template>
  <todo-list></todo-list>
</template>

<script>
import TodoList from "./components/TodoList.vue";

export default {
  name: "App",
  components: {
    TodoList,
  },
};
</script>

<style>
  .container {
    padding: 10px;
    margin: 10px;
    border: 1px solid #ccc;
    border-radius: 10px;
    box-shadow: 5px 5px 5px rgb(216, 216, 216), -5px -5px 5px rgb(216, 216, 216);
  }

  .nav {
    display: flex;
    justify-content: space-around;
  }

  button {
    display: inline-block;
    padding: 5px;
    background: green;
    border: none;
  }
</style>
