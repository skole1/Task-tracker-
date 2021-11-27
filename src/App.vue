<template>
  <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </div>
  <div class="container">
    <div class="card p-4">
      <Header @toggle-add-task="toggleAddTask" :showAddTask="showAddTask" title="Task Tracker"/>
      <div v-if="showAddTask">
        <AddTask @add-task="addTask" />
      </div>
      <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
    </div>
    <Footer />
  </div>
  <router-view/>
</template>

<script>
import Header from './components/Header'
import Footer from './components/Footer'
import AddTask from './components/AddTask'
import Tasks from './components/Tasks'
import { shallowReactive } from '@vue/reactivity'


export default {
  name:'App',
  props:{
   
    },
  components:{
    Header,
    Footer,
    Tasks,
    AddTask
  },
  data(){
    return {
      tasks:[],
      showAddTask: false
    }
  },

  methods:{
    toggleAddTask(){
      this.showAddTask = !this.showAddTask
    },
    async addTask(task){
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type':'application/json'
        },
        body: JSON.stringify(task)
      })

      const data = await res.json()
      this.tasks = [...this.tasks, data]
    },

    async deleteTask(id){
      if(confirm('Are you sure?')){
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE'
        })

        res.status === 200 ? (this.tasks = this.tasks.filter((task)=> task.id !== id)) : alert('Error deleting task')
      }
    },

    async toggleReminder(id){
      const tasksToToggle = await this.fetchTask(id)
      const updTask = {...tasksToToggle, reminder: !tasksToToggle.reminder }

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type':'application/json'
        },
        body: JSON.stringify(updTask)
      })

      const data = await res.json()

      this.tasks = this.tasks.map((task)=> task.id === id ? {...task, reminder: data.reminder}: task)
    },
     async fetchTasks(){
       const res = await fetch('api/tasks')

       const data = await res.json()

       return data
     },
     async fetchTask(id){
       const res = await fetch(`api/tasks/${id}`)

       const data = await res.json()

       return data
     }
  },
  async created(){
    this.tasks = await this.fetchTasks()
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
