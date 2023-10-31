<template>
	<div class="app">
	  <h1>Task List</h1>
	  <section class="task-creation">
		<h2>Create a Task</h2>
		<input v-model="newTask" @keydown.enter="addTask" placeholder="Enter a new task" class="input" />
		<button @click="addTask" class="button">Add Task</button>
		<p class="error" v-if="error">{{ error }}</p>
		<input v-model="newStatus" @keydown.enter="addCustomStatus" placeholder="Enter a custom status" class="input" />
		<button @click="addCustomStatus" class="button">Add Custom Status</button>
	  </section>
	  <section class="task-list">
		<h2>Task List</h2>
		<div class="filter-dropdown-list">
		  <label>
			Filter Tasks by Status:
			<select v-model="filter" class="filter-dropdown">
			  <option value="all">All</option>
			  <option value="completed">Completed</option>
			  <option value="incomplete">Incomplete</option>
			  <option v-for="customStatus in customStatuses" :key="customStatus">{{ customStatus }}</option>
			</select>
		  </label>
		</div>
		<ul>
		  <li v-for="task in filteredTasks" :key="task.id" class="task">
			<input type="checkbox" v-model="task.completed" class="checkbox" />
			<span :class="{ completed: task.completed }" class="task-text">{{ task.text }}</span>
			<button @click="deleteTask(task.id)" class="delete-button">Delete</button>
			<div v-if="task.editing" class="task-status-dropdown">
			  <select v-model="task.status" class="status-dropdown">
				<option value="Default">Default</option>
				<option v-for="customStatus in customStatuses" :key="customStatus">{{ customStatus }}</option>
			  </select>
			</div>
			<button @click="toggleTaskStatus(task)" class="edit-button">Edit Status</button>
		  </li>
		</ul>
	  </section>
	</div>
  </template>
  
  <script>
  export default {
	data() {
	  return {
		newTask: "",
		newStatus: "",
		tasks: [],
		filter: "all",
		error: "",
		customStatuses: [],
	  };
	},
	computed: {
	  filteredTasks() {
		if (this.filter === "all") {
		  return this.tasks;
		}
		return this.tasks.filter((task) => {
		  if (this.filter === "completed" && task.completed) {
			return true;
		  }
		  if (this.filter === "incomplete" && !task.completed) {
			return true;
		  }
		  if (this.customStatuses.includes(this.filter) && task.status === this.filter) {
			return true;
		  }
		  return false;
		});
	  },
	},
	methods: {
	  addTask() {
		if (this.newTask.trim() === "") {
		  this.error = "Task title cannot be empty";
		  return;
		} else {
		  this.error = "";
		}
  
		this.tasks.push({
		  id: Date.now(),
		  text: this.newTask,
		  status: "Default",
		  completed: false,
		  editing: false,
		});
		this.newTask = "";
	  },
	  deleteTask(taskId) {
		const index = this.tasks.findIndex((task) => task.id === taskId);
		if (index !== -1) {
		  this.tasks.splice(index, 1);
		}
	  },
	  toggleTaskStatus(task) {
		task.editing = !task.editing;
	  },
	  addCustomStatus() {
		if (this.newStatus.trim() === "") {
		  this.error = "Custom status cannot be empty";
		  return;
		} else {
		  this.error = "";
		}
  
		this.customStatuses.push(this.newStatus);
		this.newStatus = "";
	  },
	},
  };
  </script>
  
  <style>
  .completed {
	text-decoration: line-through;
  }
  
  .filter-dropdown {
	padding: 5px;
  }
  
  .filter-dropdown-list {
	margin-left: 20px;
	margin-top: 20px;
  }
  
  .task-status-dropdown select {
	width: 100%;
	padding: 5px;
	border: 1px solid #ccc;
	border-radius: 4px;
	margin-top: 5px;
  }
  
  .edit-button {
	background-color: #007BFF;
	color: #fff;
	border: none;
	padding: 5px 10px;
	cursor: pointer;
	border-radius: 4px;
	margin-left: 10px;
  }
  
  .button {
	background-color: #007BFF;
	color: #fff;
	border: none;
	padding: 5px 10px;
	cursor: pointer;
	border-radius: 4px;
	margin-top: 5px;
  }
  
  .delete-button {
	background-color: #FF0000;
	color: #fff;
	border: none;
	padding: 5px 10px;
	cursor: pointer;
	border-radius: 4px;
	margin-top: 5px;
  }
  
  .task {
	display: flex;
	align-items: center;
  }
  
  .task:not(:last-child) {
	margin-bottom: 10px;
  }
  
  .task:hover {
	background-color: #f5f5f5;
  }
  </style>
  
