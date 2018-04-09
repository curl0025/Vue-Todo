<template>
  <div>
    <h2>{{listTitle}}</h2>
    <div class="toolbar">
      <label for="priority-filter">Show priority</label>
      <select name="priority-filter" id="priority-filter" v-model="selectedPriority">
        <option value="">all</option>
        <option
          v-for="option in priorityOptions"
          :value="option"
          :key="option">{{option}}</option>
      </select>
      <label for="category-filter">Show category</label>
      <select name="category-filter" id="category-filter" v-model="selectedCategory">
        <option value="">all</option>
        <option
          v-for="option in categoryOptions"
          :value="option"
          :key="option">{{option}}</option>
      </select>
    </div>
    <ul class="task-list">
      <li v-for="task in filteredTasks" :key="task.id" class="task-item">
        <span class="far"
          :class="{
            'fa-circle': ! task.isComplete,
            'fa-check-circle': task.isComplete
          }"
          @click="toggleDone(task)"></span>
          <div class="the-title">
        <span>{{ task.title }}</span>
          </div>
          <div class="priority">
        <span>{{ task.priority }}</span>
          </div>
          <div class="the-option">
        <span>{{ task.category }}</span>
          </div>
        <span class="far fa-trash-alt" @click="removeTask(task)"></span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  // We are not actually managing the task list in this component.
  // We are only displaying the list. The parent component (App.vue) is
  // passing down a reference to the list in the 'tasks' prop.
  props: ['tasks', 'listTitle'],
  data () {
    return {
      priorityOptions: ['Low', 'Medium', 'High'],
      categoryOptions: ['Home', 'School', 'Work'],
      selectedPriority: '',
      selectedCategory: ''
    }
  },
  computed: {
    filteredPriorityTasks () {
      // If the selectedFilter is 'all', the value is set to an empty string
      // which will evaluate to false. In that case, we will reuturn the
      // entire task list. Otherwise we use the array.filter() method to return
      // only the tasks with the selected priority.
      return (!this.selectedPriority)
        ? this.tasks
        : this.tasks.filter(task => task.priority === this.selectedPriority)
    },
    filteredCategoryTasks () {
      return (!this.selectedCategory)
        ? this.tasks
        : this.tasks.filter(task => task.category === this.selectedCategory)
    },
    filteredTasks () {
      const combined = [
        ...this.filteredPriorityTasks,
        ...this.filteredCategoryTasks
      ]
      const merged = [...new Set(combined)]
      return merged.filter(task => 
        this.filteredPriorityTasks.includes(task) &&
        this.filteredCategoryTasks.includes(task)
      )
    }
  },
  methods: {
    // Since this component does not own the task list data, we need to
    // notify the parent component that the user wants to toggle the
    // completion status of the task.  We do that by emitting a custom event,
    // and passing up the affected task with the event. We will listen for
    // this event in the parent component and call the appropriate method to
    // make the change to the data.
    toggleDone (task) {
      this.$emit('toggleDone', task)
    },
    removeTask (task) {
      this.$emit('removeTask', task)
    }
  }
}
</script>
<style>

/* We will add styles here later. */
.task-item {
 max-width: 400px;
 height: 90px;
 border: 2px solid #42b883;
 border-radius: 5px;
 margin: 0 auto;
 margin-top: 50px;
 
 
}
.toolbar {
  margin-bottom: 1rem;
}

h1 {
  text-align: center;
  
}

h2 {
  text-align: center;
  color: #35495e;
  
}

section {
  text-align: center;
}

.far {
  float: right;
 
}

.the-title {
 text-align: center;
 margin-bottom: 10px;
 

}

.priority {
  text-align: center;
  
}


.the-option {
  text-align: center;
  margin-top: 5px;
 
}

select {
  border: 2px solid #35495e;
  
  cursor: pointer;
  
}

.task-title {
  border: 2px solid #35495e;


}







</style>
