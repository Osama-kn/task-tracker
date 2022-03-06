<template>
  <AddTask
    v-show="showAddTask"
    @toggle-add-task="toggleAddTask"
    @add-task="addTask"
  />

  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script lang="ts">
import Tasks from "../components/Tasks.vue";
import AddTask from "../components/AddTask.vue";

import { defineComponent } from "vue";

export default defineComponent({
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [] as any,
    };
  },
  methods: {
    async addTask(task: Object) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(task),
      });

      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id: any) {
      if (confirm("Are you sure you want to delete this")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });
        res.status === 200
          ? (this.tasks = this.tasks.filter((task: any) => task.id !== id))
          : alert("Error deleting task");
      }
    },
    async toggleReminder(id: any) {
      const taskToggle = await this.fetchTask(id);
      const updTask = { ...taskToggle, reminder: !taskToggle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(updTask),
      });
      const data = await res.json();
      this.tasks = this.tasks.map((task: any) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    async fetchTasks() {
      const res = await fetch("api/tasks");
      const data = await res.json();
      return data;
    },
    async fetchTask(id: any) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
});
</script>
