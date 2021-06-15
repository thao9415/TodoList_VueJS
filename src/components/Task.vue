<template>
  <div class="container">
    <div class="task">
      <!-- title -->
      <div class="title">
        <h1>To Do List</h1>
      </div>
      <!-- form -->
      <div class="form">
        <input type="text" placeholder="New Task" v-model="newTask" @keyup.enter="addTask"/>
        <button @click="addTask"><i class="fas fa-plus"></i></button>
      </div>
      <!-- task lists -->
      <div class="taskItems">
        <ul>
          <TaskItem v-for="(task, index) in tasks" :key="task.id"
                    v-bind:task="task"
                    @remove="removeTask(index)"
                    @complete="completeTask(task)"
          ></TaskItem>
        </ul>
      </div>
      <!-- buttons -->
      <div class="clearBtns">
        <button v-on:click="clearCompleted">Clear completed</button>
        <button v-on:click="clearAll">Clear all</button>
      </div>
      <!-- pending task -->
      <div class="pendingTasks">
        <span>Pending Tasks: {{ incomplete }} </span>
      </div>
    </div>
  </div>
</template>

<script>
import TaskItem from "@/components/TaskItem";

export default {
  name: "Task",
  components: {TaskItem},
  props: ['tasks'],
  computed: {
    incomplete() {
      return this.tasks.filter(this.inProgress).length;
    }
  },
  data() {
    return {
      newTask: '',
    }
  },
  methods: {
    addTask() {
      if (this.newTask) {
        this.tasks.push({
          title: this.newTask,
          completed: false
        });
        this.newTask = "";
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    completeTask(task) {
      task.completed = !task.completed;
    },
    clearAll() {
      this.tasks = [];
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(this.inProgress)
    },
    isCompleted(task) {
      return task.completed;
    },
    inProgress(task) {
      return this.isCompleted(task);
    }
  },
};
</script>
