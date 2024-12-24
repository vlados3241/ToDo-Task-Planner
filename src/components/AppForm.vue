<template>
    <div>
      <form @submit.prevent>
        <div v-if="isCreating">
          <h3>Создать задачу</h3>
          <input
            v-model="task.title"
            class="input"
            type="text"
            placeholder="Название"/>
            
          <input
            v-model="task.body"
            class="input"
            type="text"
            placeholder="Описание"/>

          <button
            :disabled="!task.title && !task.body"
            class="btn"
            @click="submitTask">
            Создать
          </button>
        </div>
  
        <div v-else>
          <h3>Редактировать задачу</h3>
          <input
            v-model="task.title"
            class="input"
            type="text"
            placeholder="Название"/>

          <input
            v-model="task.body"
            class="input"
            type="text"
            placeholder="Описание"/>

          <button
            :disabled="!task.title && !task.body"
            class="btn"
            @click="submitTask">
            Редактировать
          </button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      task: {
        type: Object,
        required: true,
        default: () => ({ title: '', body: '', id: null }),
      },
      isCreating: {
        type: Boolean,
        required: true,
      },
    },
    methods: {
      submitTask() {
        if (this.isCreating) {
          this.task.id = Date.now();
          this.$emit('create', this.task);
        } else {
          this.$emit('edit', this.task);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  form {
    display: flex;
    flex-direction: column;
  }
  .input {
    border-radius: 5px;
    width: 66%;
    padding: 10px 15px;
    margin-top: 10px;
    margin-left: 10px;
    border: 0;
    border: 1px solid black;
  }
  .btn {
    align-self: flex-start;
    margin: 15px 10px 10px;
    padding: 10px 15px;
    color: white;
    background-color: #3963fa;
    border: 0;
    border-radius: 10px;
  }
  h3 {
    margin-left: 10px;
    margin-top: 10px;
    margin-bottom: 10px;
  }
  </style>
  