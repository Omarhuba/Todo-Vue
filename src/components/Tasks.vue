<template>
  <div v-for="task in tasks" :key="task.id">
    <Task :task="task" @delete-task="$emit('delete-task', task.id )" @toggle-reminder="$emit('toggle-reminder', task.id)"/>
  </div>
</template>

<script>
import Task from './Task.vue'
export default {
    name: 'TasksC',
    props: {
        tasks: Array,
    },
    components: {Task},
    emits: ['delete-task', 'toggle-reminder']
}
</script>

<style scoped>

</style>