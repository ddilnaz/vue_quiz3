<template>
  <div id="app">
    <header>
      <h1>Animated To-Do List</h1>
    </header>
    <main>
      <TodoList />
    </main>
    <footer>
      <p>Made with love </p>
    </footer>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import TodoList from './components/TodoList.vue';

export default defineComponent({
  name: 'App',
  components: {
    TodoList, 
  },
});
</script>

<style scoped>
#app {
  font-family: 'Arial', sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  background: linear-gradient(145deg, #ff88b1, #ff80bf);
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

header {
  background-color: #ff60a1;
  color: white;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

footer {
  margin-top: 20px;
  font-size: 0.9em;
  color: #555;
}

footer p {
  color: #ff4d7d;
}

h1 {
  font-family: 'Roboto', sans-serif;
  font-weight: 700;
  font-size: 2rem;
  text-transform: uppercase;
  letter-spacing: 2px;
}

button {
  padding: 12px 25px;
  border-radius: 30px;
  background-color: #ff60a1;
  color: white;
  font-weight: bold;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
}

button:hover {
  background-color: #ff4d7d;
  transform: scale(1.05);
}

button:focus {
  outline: none;
}

.task-list {
  list-style-type: none;
  padding: 0;
}

.task-list li {
  background-color: #fff;
  color: #333;
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-list li.completed {
  text-decoration: line-through;
  color: #999;
}

.task-list li.high {
  border-left: 5px solid #ff4d7d;
}

.task-input input,
.task-input select {
  padding: 12px;
  border-radius: 25px;
  border: 2px solid #ff60a1;
  margin: 10px 0;
  width: 80%;
  font-size: 1rem;
}

.task-input input:focus,
.task-input select:focus {
  outline: none;
  border-color: #ff4d7d;
}

.task-input {
  margin-bottom: 20px;
}
</style>
