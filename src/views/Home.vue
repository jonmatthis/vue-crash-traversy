<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import AddTask from "@/components/AddTask.vue";
import Tasks from "@/components/Tasks.vue";
export default {
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
      tasks: [],
    };
  },
  methods: {
    async addTask(newTask) {
      console.log("Adding task: ", newTask);
      const response = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(newTask),
      });
      const data = await response.json();
      this.tasks = [...this.tasks, data];
      this.showAddTask = false;
    },
    async deleteTask(id) {
      console.log("deleting task ", id);
      const response = await fetch(`api/tasks/${id}`, {
        method: "DELETE",
      });

      response.status === 200
        ? (this.tasks = this.tasks.filter((task) => task.id !== id))
        : alert("Error occurred when deleting task");
    },

    async toggleReminder(id) {
      console.log("toggling reminder for task ", id);
      const taskToToggle = await this.fetchTask(id);
      const updatedTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const response = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updatedTask),
      });

      const data = response.json();

      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },

    async fetchTasks() {
      console.log(`Fetching tasks...`);

      const response = await fetch("api/tasks");
      return response.json();
    },
    async fetchTask(id) {
      console.log(`Fetching task with id: ${id}`);
      const response = await fetch(`api/tasks/${id}`);
      return response.json();
    },
  },
  async created() {
    console.log("App created!");
    this.tasks = await this.fetchTasks();
  },
};
</script>
