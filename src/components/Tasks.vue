<template>
  <header>
    <div :key="task.id" v-for="task in tasks">
      <Task @toggle-reminder="$emit('toggle-reminder', task.id)"
            @delete-task="$emit('delete-task', task.id)"
            :task="task"
      />
    </div>
  </header>
</template>

<script>
import Task from './Task.vue'

export default {
  name: 'Tasks',
  props: {
    tasks: {
      type: Array,
      default: []
    }
  },
  components: {
    Task
  },
  emits: ['delete-task', 'toggle-reminder']
}
</script>
