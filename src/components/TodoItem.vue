<template>
    <li :class="{ completed: task.completed, high: task.priority === 'high' }">
      <span @click="toggle">{{ task.title }}</span>
      <span>({{ task.priority }})</span>
      <button @click="onDelete">Delete</button>
    </li>
  </template>
  
  <script  lang="ts">
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
      onDelete() {
        this.$emit('delete', this.task.id);
      },
      toggle() {
        this.$emit('toggle', this.task.id);
      },
    },
  });
  </script>
  
  <style scoped>
  li {
    display: flex;
    justify-content: space-between;
    padding: 10px;
    margin: 5px 0;
    border: 1px solid #ddd;
    border-radius: 5px;
    transition: background 0.3s;
  }
  
  li.completed {
    text-decoration: line-through;
    color: gray;
  }
  
  li.high {
    background-color: #fdd;
  }
  </style>
  