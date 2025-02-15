<template>
  <div>
    <header>
      <h2>To-Do List</h2>
      <p>{{ pendingTasks }} task(s) pending</p>
    </header>
    <div class="task-input">
      <input
        v-model="newTaskTitle"
        placeholder="Enter task title"
        @keyup.enter="addTask"
      />
      <select v-model="newTaskPriority">
        <option value="low">Low</option>
        <option value="medium">Medium</option>
        <option value="high">High</option>
      </select>
      <button @click="addTask">Add Task</button>
    </div>
    <transition-group name="task" tag="ul" class="task-list">
      <li
        v-for="task in sortedTasks"
        :key="task.id"
        :class="{ completed: task.completed, high: task.priority === 'high' }"
      >
        <TodoItem
          :task="task"
          @delete="deleteTask"
          @toggle="toggleTaskCompletion"
        />
      </li>
    </transition-group>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, nextTick } from 'vue';
import { Task } from '../interfaces/Task';
import TodoItem from './TodoItem.vue';

export default defineComponent({
  name: 'TodoList',
  components: { TodoItem },
  setup() {
    const tasks = ref<Task[]>([]);
    const newTaskTitle = ref('');
    const newTaskPriority = ref<'low' | 'medium' | 'high'>('low');

    const priorityRank = {
      low: 1,
      medium: 2,
      high: 3,
    };

    const addTask = async () => {
      if (!newTaskTitle.value.trim()) return;
      const newTask: Task = {
        id: Date.now(),
        title: newTaskTitle.value,
        priority: newTaskPriority.value,
        completed: false,
      };
      tasks.value.push(newTask);
      newTaskTitle.value = '';
      newTaskPriority.value = 'low';
      
      nextTick(() => {
        scrollToNewTask();
      });
    };

    const deleteTask = (id: number) => {
      tasks.value = tasks.value.filter(task => task.id !== id);
    };

    const toggleTaskCompletion = (id: number) => {
      const task = tasks.value.find(task => task.id === id);
      if (task) {
        task.completed = !task.completed;
        tasks.value = [...tasks.value]; 
      }
    };

    const sortedTasks = computed(() =>
      tasks.value.slice().sort((a, b) => priorityRank[b.priority] - priorityRank[a.priority])
    );

    const pendingTasks = computed(() =>
      tasks.value.filter(task => !task.completed).length
    );

    const scrollToNewTask = () => {
      const taskList = document.querySelector('.task-list');
      if (taskList) taskList.scrollTo({ top: taskList.scrollHeight, behavior: 'smooth' });
    };

    return {
      tasks,
      newTaskTitle,
      newTaskPriority,
      addTask,
      deleteTask,
      toggleTaskCompletion,
      sortedTasks,
      pendingTasks,
    };
  },
});
</script>

<style scoped>
.task-input {
  margin-bottom: 20px;
}

input {
  padding: 10px;
  margin-right: 10px;
  width: 60%;
  border-radius: 5px;
  border: 1px solid #f8c8d8; 
  background-color: #f9e4f2;
}

select {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #f8c8d8;
  background-color: #f9e4f2;
}

button {
  padding: 10px 20px;
  border-radius: 5px;
  background-color: #ff66b2; 
  color: white;
  cursor: pointer;
  border: none;
}

button:hover {
  background-color: #ff3385; 
}

.task-list {
  list-style-type: none;
  padding: 0;
}

.task-list li {
  padding: 15px;
  margin-bottom: 10px;
  background-color: #ffe6f1; 
  border-radius: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.task-list li:hover {
  background-color: #ffb3d9; 
}

.task-list li.completed {
  text-decoration: line-through;
  color: #ffb3d9; 
}

.task-list li.high {
  border-left: 5px solid #ff3385; 
}

button {
  background-color: #ff66b2;
  color: white;
}

button:hover {
  background-color: #e63946;
}
</style>
