<template>
  <header>
    <h1>{{title}}</h1>
    <!-- <h1>Task-Tracker</h1> -->
    <Button :text="showAddTask ? 'Close' : 'Add Task'" :color="showAddTask ? 'red': 'green'" @btn-click="$emit('toggle-add-task')"/>
  </header>
</template>

<script>
import Button from './Button.vue'
export default {
    name: 'HeaderComponent',
    data(){return{

    }},
    components: {Button},
    props: {
        title: String,
        showAddTask: Boolean,
    }
}
</script>

<style scoped style:{scss}>
    header{
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>