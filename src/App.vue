<template>
  <div id="app">
    <!-- We need some navigation links to switch between views -->
   
   <div class="header">
    <h1 class="title">Vue Todos</h1>
   </div>
    
    <nav class="toggle">
      <ul>
        <!--
          The RouterLink component comes with VueRouter.
          The 'to' prop is the URL route.
        -->
        <div class="all">
        <li><router-link to="/">All</router-link></li>
        </div>
        <div class="active">
        <li><router-link to="/active">Active</router-link></li>
        </div>
        <div class="completed">
        <li><router-link to="/completed">Completed</router-link></li>
        </div>
        
      </ul>
    </nav>
    
    <add-task-form @taskAdded="addTask" />
    <!--
      The RouterView is a placeholder that VueRouter uses to know
      where to insert the designated component for a given URL.
      In this case we are passing our taskList as a prop to each route's
      component and we are listening for user events.  This way we are still
      only mainting one master list in the App.vue component.
     -->
    <router-view
      :tasks="taskList"
      @toggleDone="toggleDone"
      @removeTask="removeTask"
    />
  </div>
</template>

<script>
import AddTaskForm from '@/components/AddTaskForm'

export default {
  name: 'App',
  components: { AddTaskForm },
  data () {
    return {
      taskList: [
        // { id: 1234, title: 'Learn Vue', isComplete: true, priority: 'medium' },
        // { id: 1235, title: 'Learn Vue Router', isComplete: false, priority: 'medium' },
        // { id: 1236, title: 'Learn Vuex', isComplete: false, priority: 'medium' },
        // { id: 1237, title: 'Learn Vue DevTools', isComplete: true, priority: 'medium' }
      ]
    }
  },

  created () {
    this.taskList = JSON.parse(localStorage.getItem('taskList')) || []
  },

  methods: {
    addTask (task) {
      this.taskList.push(task)
      this.saveTaskList()
    },

    toggleDone (task) {
      task.isComplete = !task.isComplete
      this.saveTaskList()
    },

    removeTask (task) {
      const taskIndex = this.taskList.indexOf(task)
      this.taskList.splice(taskIndex, 1)
      this.saveTaskList()
    },

    saveTaskList () {
      localStorage.setItem('taskList', JSON.stringify(this.taskList))
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  width: 100%;
  max-width: 550px;
  margin: 60px auto;
}
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  
  
}
nav ul {
  display: flex;
  justify-content: flex-start;
}
nav ul li {
  
  text-align: center;
  margin: 0 auto;
  margin-bottom: 50px;
  
  
}

.header {
  background: #42b883;
  width: 100%;
}


h1 {
  color:#35495e;
  text-align: center;
}


.all {
  
  border: 2px solid black;
  border-radius: 20px;
  height: 20px;
  width: 90px;
  background: #42b883;
}

.active {
  
  border: 2px solid black;
  border-radius: 20px;
  height: 20px;
  width: 90px;
  background: #42b883;
  cursor: pointer;
}

.completed {
  
  border: 2px solid black;
  border-radius: 20px;
  height: 20px;
  width: 90px;
  background: #42b883;
  cursor: pointer;
}

.toggle {
  width: 300px;
  height: 40px;
  margin: 0 auto;
  margin-bottom: 50px;
  cursor: pointer;
  
  
}


</style>
