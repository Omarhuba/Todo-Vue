<template>
  <div class="container">
    <Header title="Task Tracker"  @toggle-add-task="toggleAddTask" :showAddTask="showAddTask"/>
    <div v-show="showAddTask">
      <AddTask  @add-task="addTask" />
    </div>
    <Tasks :tasks="tasks" @delete-task="deleteTask" @toggle-reminder="toggleReminder"/>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";
import axios from 'axios'
export default {
  name: "App",
  data() {
    return {
      tasks: [],
      showAddTask: false
    };
  },
  methods:{
    async addTask(task){
      const res = await axios.post('api/tasks', task )

      this.tasks = [...this.tasks,  res.data ]
    },


    async deleteTask(id){
      if(confirm('Are you sure?')){

        const res = await axios.delete(`api/tasks/${id}`, id)
        res.status === 200 ? (this.tasks = this.tasks.filter((task)=> task.id !== id)) : alert('Error....')

      }
    },
    // async toggleReminder(id) {
    //   const taskToToggle = await this.fetchTask(id)
    //   const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder }
    //   const res = await fetch(`api/tasks/${id}`, {
    //     method: 'PUT',
    //     headers: {
    //       'Content-type': 'application/json',
    //     },
    //     body: JSON.stringify(updTask),
    //   })
    //   const data = await res.json()
    //   this.tasks = this.tasks.map((task) =>
    //     task.id === id ? { ...task, reminder: data.reminder } : task
    //   )
    // },
    async toggleReminder(id){
      const taskToggle = await this.fetchTask(id)
      const updTask = {...taskToggle, reminder: !taskToggle.reminder};
      const res= await axios.put(`api/tasks/${id}`, updTask, id)
        //mycket viktig med map function......
        this.tasks = this.tasks.map((task)=> task.id === id ? {...task, reminder: res.data.reminder} : task)

      // const res = await fetch(`api/tasks/${id}`, {
      //   method: 'PUT',
      //   headers: {
      //     'Content-type': 'application/json'
      //   },
      //   body: JSON.stringify(updTask)
      // })
      // const data = await res.json()
      // console.log(data);
    },
    // async toggleReminder(id){
    //   const taskToggle = await this.fetchTask(id)
    //   const updTask = {...taskToggle, reminder: !taskToggle.reminder }
    //   const res = await fetch(`api/tasks/${id}`, {
    //      method: 'PUT',
    //       headers: {
    //         'Content-type': 'application/json',
    //       },
    //       body: JSON.stringify(updTask)
    //   })
    //   const data = await res.json()
    //   //mycket viktig med map function......
    //   this.tasks = this.tasks.map((task)=> task.id === id ? {...task, reminder: data.reminder} : task)
    // },


      toggleAddTask(){
        this.showAddTask = !this.showAddTask
      },
      async fetchTasks(){
        const res = await axios.get('api/tasks')
        // const data = await res.json()
        return res.data
      },


      async fetchTask(id){
        const res = await axios.get(`api/tasks/${id}`)
        // const data = await res.json()
        return res.data
      }
  },
  async created() {
    this.tasks = await this.fetchTasks()
    // try{
    //   const res = await axios.get('api/tasks')
    //   this.tasks = res.data
    // }catch(e){
    //   console.log(e.message);
    // }
  },
  components: { Header, Tasks, AddTask },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
