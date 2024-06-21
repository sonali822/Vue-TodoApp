<template>
  <div id="app">
    <h1>Todo List</h1>
    <div class="filters">
      <label for="status-filter">Filter by Status:</label>
      <select v-model="selectedFilter" id="status-filter" @change="handleFilterChange()">
        <option value="all">All</option>
        <option value="todo">To Do</option>
        <option value="in-progress">In Progress</option>
        <option value="done">Done</option>
      </select>
    </div>

    <div class="card">
      <input type="text" v-model="title" placeholder="Title...">
      <input type="text" v-model="description" placeholder="Description...">
      <button class="add-button" @click="addTask()">Add Task</button>
    </div>
    <div class="tasks-card" v-if="filteredTasks.length > 0">
      <div class="tasks">
        <div v-for="(task, index) in filteredTasks" :key="task.id" class="task">
          <input type="checkbox" v-model="task.completed" @change="toggleTask(index)">
          <div class="task-details">
            <h3>{{ task.title }}</h3>
            <p>{{ task.description }}</p>
          </div>
          <div class="task-buttons">
            <button @click="editTask(index)">Edit</button>
            <button @click="deleteTask(index)">Delete</button>
          </div>
        </div>
      </div>
    </div>


  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      title: '',
      description: '',
      tasks: [],
      nextId: 1,
      selectedFilter: 'all',
    };
  },
  computed: {
    filteredTasks() {
      if (this.selectedFilter === 'all') {
        return this.tasks;
      } else if (this.selectedFilter === 'todo') {
        return this.tasks.filter(task => !task.completed);
      } else if (this.selectedFilter === 'in-progress') {
        return this.tasks.filter(task => !task.completed);
      } else if (this.selectedFilter === 'done') {
        return this.tasks.filter(task => task.completed);
      }


      return this.tasks;
    }
  },

  methods: {
    addTask() {
      if (this.title.trim() !== '' && this.description.trim() !== '') {
        this.tasks.push({
          id: this.nextId++,
          title: this.title.trim(),
          description: this.description.trim(),
          completed: false,
        });
        this.title = '';
        this.description = '';
      }
    },
    handleFilterChange() {
      console.log('Selected filter changed to:', this.selectedFilter);
    },
    editTask(index) {
      const newTitle = prompt('Enter new title', this.tasks[index].title);
      const newDescription = prompt('Enter new description', this.tasks[index].description);
      if (newTitle !== null && newDescription !== null) {
        this.tasks[index].title = newTitle.trim();
        this.tasks[index].description = newDescription.trim();
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    toggleTask(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
    },
  },
};
</script>

<style>
.filters {
  margin-bottom: 10px;
}

.tasks-card {
  margin-top: 20px;
  width: 100%;
  max-width: 600px;
  box-sizing: border-box;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 20px;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  background-color: #f0f0f0;
}

.card {
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 20px;
  width: 100%;
  max-width: 400px;
  box-sizing: border-box;
}

.card input {
  width: calc(100% - 40px);
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

.tasks {
  margin-top: 20px;
}

.task {
  display: flex;
  align-items: center;
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 5px;
}

.task-details {
  flex: 1;
  background-color: white;
  padding: 10px;
}

.task-details input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
  border: 1px solid #ccc;
}

.add-button {
  background-color: rebeccapurple;
  color: white;
}

.task-buttons {
  display: flex;
  align-items: center;
}

.task-buttons button {
  margin-left: 5px;
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
}
</style>
