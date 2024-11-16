<template>
  <div id="app">
    <header>
      <h1>Animated To-Do List</h1>
    </header>
    <main>
      <TodoList />
    </main>
    <footer>
      <p>Made with ❤️ using Vue.js</p>
    </footer>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import TodoList from './components/TodoList.vue';
import TodoItem from './components/TodoItem.vue';

export default defineComponent({
  name: 'App',
  components: {
    TodoList,
    TodoItem,
  },
});
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

header {
  background-color: #4caf50;
  color: white;
  padding: 10px 0;
  border-radius: 5px;
  margin-bottom: 20px;
}

footer {
  margin-top: 20px;
  font-size: 0.9em;
  color: #555;
}
</style>
