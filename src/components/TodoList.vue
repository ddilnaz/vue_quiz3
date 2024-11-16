<template>
  <div class="todo-list">
    <form @submit.prevent="addTask">
      <input v-model="newTaskTitle" placeholder="Task title" required />
      <select v-model="newTaskPriority">
        <option value="low">Low</option>
        <option value="medium">Medium</option>
        <option value="high">High</option>
      </select>
      <button type="submit">Add Task</button>
    </form>

    <transition-group name="list" tag="ul" class="task-list">
      <TodoItem
        v-for="task in sortedTasks"
        :key="task.id"
        :task="task"
        @delete="deleteTask"
        @toggle="toggleTaskCompletion"
      />
    </transition-group>

    <p>Pending tasks: {{ pendingTasks }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, nextTick, watch } from 'vue';
import { Task } from '../interfaces/Task';

import TodoItemm from '@/components/TodoItem.vue';


export default defineComponent({
  name: 'TodoList',
  components: { TodoItemm },
  setup() {
    const tasks = ref<Task[]>([]);
    const newTaskTitle = ref('');
    const newTaskPriority = ref<'low' | 'medium' | 'high'>('low');

    // Priority ranking for sorting
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
      await nextTick(() => scrollToNewTask());
    };

    const deleteTask = (id: number) => {
      tasks.value = tasks.value.filter(task => task.id !== id);
    };

    const toggleTaskCompletion = (id: number) => {
      const task = tasks.value.find(task => task.id === id);
      if (task) {
        task.completed = !task.completed;
        // Ensuring reactivity
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

    watch(tasks, () => console.log('Task list updated'));

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
.todo-list {
  padding: 20px;
}

form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.task-list {
  list-style: none;
  padding: 0;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

.task-list li.completed {
  text-decoration: line-through;
  opacity: 0.6;
}

button:focus {
  outline: 2px solid #4f9;
}
</style>
