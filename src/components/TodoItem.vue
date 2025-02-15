<template>
  <div class="todo-item">
    <span
      :class="{ completed: task.completed }"
      @click="toggleTaskCompletion"
    >
      {{ task.title }} ({{ task.priority }})
    </span>
    <button @click="deleteTask">Delete</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { Task } from '../interfaces/Task';

export default defineComponent({
  name: 'TodoItem',
  props: {
    task: {
      type: Object as PropType<Task>,
      required: true,
    },
  },
  emits: ['delete', 'toggle'],
  methods: {
    deleteTask() {
      this.$emit('delete', this.task.id);
    },
    toggleTaskCompletion() {
      this.$emit('toggle', this.task.id);
    },
  },
});
</script>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  padding: 15px;
  margin-bottom: 12px;
  background-color: #fff1f9; 
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.todo-item:hover {
  background-color: #ffe6f2; 
  transform: scale(1.02); 
}

span {
  font-size: 1.1rem;
  color: #ff66b2;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
}

span.completed {
  text-decoration: line-through;
  color: #ffb3d9; 
}

button {
  margin-left: 20px;
  padding: 8px 15px;
  background-color: #ff66b2; 
  color: white;
  font-weight: bold;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

button:hover {
  background-color: #ff3385; 
  transform: scale(1.05); 
}

button:focus {
  outline: none; 
}
</style>
