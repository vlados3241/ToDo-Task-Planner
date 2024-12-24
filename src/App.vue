<template>
  <div class="app">
    <h1 style="display: flex; justify-content: center;">ToDo Планировщик задач</h1>

    <button class="modalBtn" @click="openCreateModal">Добавить задачу</button>

    <AppModalWindow v-model:show="modalVisible">
      <AppForm
        :task="currentTask"
        :is-creating="isCreating"
        @create="createTask"
        @edit="updateTask"
      />
    </AppModalWindow>

    <transition-group name="task" class="task-list" v-if="tasks.length > 0">
      <AppTaskList
        v-for="task in tasks"
        :key="task.id"
        class="task-list"
        :tasks="[task]"
        @edit="openEditModal"
        @remove="removeTask"
        @done="completeTask">
      </AppTaskList>
    </transition-group>

    <h1 class="listElse" v-else>Похоже все задачи выполнены!</h1>

    <button @click="goToCompletedTasks" class="modalBtn">
      Перейти к выполненным задачам
    </button>
  </div>
</template>

<script>
import AppForm from './components/AppForm.vue';
import AppTaskList from './components/AppTaskList.vue';
import AppModalWindow from './components/UI/AppModalWindow.vue';

export default {
  components: {
    AppForm,
    AppTaskList,
    AppModalWindow,
  },
  data() {
    return {
      tasks: [],
      completedTasks: [],
      modalVisible: false,
      isCreating: true,
      currentTask: null,
    };
  },
  created() {
    this.loadTasks();
  },
  methods: {
    openCreateModal() {
      this.isCreating = true;
      this.currentTask = {
        title: '',
        body: '',
      };
      this.modalVisible = true;
    },
    openEditModal(task) {
      this.isCreating = false;
      this.currentTask = { ...task };
      this.modalVisible = true;
    },
    createTask(task) {
      this.tasks.push({ ...task, id: Date.now() });
      this.saveTasks();
      this.modalVisible = false;
    },
    updateTask(updatedTask) {
      this.tasks = this.tasks.map(task =>
        task.id === updatedTask.id ? updatedTask : task
      );
      this.saveTasks();
      this.modalVisible = false;
    },
    removeTask(task) {
      this.tasks = this.tasks.filter(t => t.id !== task.id);
      this.saveTasks();
    },
    completeTask(task) {
      this.tasks = this.tasks.filter(t => t.id !== task.id);
      this.completedTasks.push({ ...task, id: Date.now() });
      this.saveTasks();
    },
    returnToMainList(task) {
      this.completedTasks = this.completedTasks.filter(t => t.id !== task.id);
      this.tasks.push({ ...task, id: Date.now() });
      this.saveTasks();
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
      localStorage.setItem('completedTasks', JSON.stringify(this.completedTasks));
    },
    loadTasks() {
      const savedTasks = JSON.parse(localStorage.getItem('tasks'));
      const savedCompletedTasks = JSON.parse(localStorage.getItem('completedTasks'));
      if (savedTasks) {
        this.tasks = savedTasks;
      }
      if (savedCompletedTasks) {
        this.completedTasks = savedCompletedTasks;
      }
    },
    goToCompletedTasks() {
      localStorage.setItem('showCompleted', true);
      window.open('/completed-tasks.html', '_blank');
    },
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
h1 {
  margin-left: 10px;
}
.modalBtn {
  margin-left: 10px;
  background-color: #3963FA;
  border-radius: 5px;
  border: 0;
  color: white;
  height: 30px;
}
.completed-task {
  text-decoration: none;
  color: #3963FA;
  font-size: 30px;
  margin-left: 10px;
  margin-top: 1px;
}
.task-list .task-enter-active, .task-list .task-leave-active {
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.task-list .task-enter, .task-list .task-leave-to {
  opacity: 0;
  transform: translateY(20px);
}
</style>
