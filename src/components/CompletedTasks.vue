<template>
    <div class="completed-tasks">
      <h2>Выполненные задачи</h2>
      <div v-if="completedTasks.length > 0">
        <div 
          class="task"
          v-for="task in completedTasks" 
          :key="task.id">
          <div>
            <div><strong>Название: </strong>{{ task.title }}</div>
            <div><strong>Описание: </strong>{{ task.body }}</div>
          </div>
        </div>
      </div>
      <div v-else>
        <p>Нет выполненных задач.</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      completedTasks: {
        type: Array,
        required: true,
      },
    },
  };
  </script>
  
  <style scoped>
  .completed-tasks {
    padding: 20px;
  }
  
  .task {
    border: 1px solid #ddd;
    padding: 10px;
    margin: 10px 0;
    border-radius: 5px;
  }
  </style>
  