<template>
	<div class="app">
	  <h1>Task List</h1>
	  <section class="task-creation">
		<h2>Create a Task</h2>
		<input
		  v-model="newTask"
		  @keydown.enter="addTask"
		  placeholder="Enter a new task"
		  class="input"
		/>
		<button @click="addTask" class="button">Add Task</button>
		<p class="error" v-if="error">{{ error }}</p>
	  </section>
	  <section class="task-filter">
		<h2>Filter Tasks</h2>
		<label>
		  <input type="radio" v-model="filter" value="all" />
		  All
		</label>
		<label>
		  <input type="radio" v-model="filter" value="completed" />
		  Completed
		</label>
		<label>
		  <input type="radio" v-model="filter" value="incomplete" />
		  Incomplete
		</label>
	  </section>
	  <section class="task-list">
		<h2>Task List</h2>
		<ul>
		  <li v-for="task in filteredTasks" :key="task.id" class="task" @click="markCompleted(task)">
			<input type="checkbox" v-model="task.completed" class="checkbox" />
			<span :class="{ completed: task.completed }" class="task-text">
			  {{ task.text }}
			</span>
			<button @click="deleteTask(task.id)" class="delete-button">Delete</button>
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
		tasks: [],
		filter: "all",
		error: "",
	  };
	},
	computed: {
	  filteredTasks() {
		if (this.filter === "completed") {
		  return this.tasks.filter((task) => task.completed);
		} else if (this.filter === "incomplete") {
		  return this.tasks.filter((task) => !task.completed);
		}
		return this.tasks;
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
		  completed: false,
		});
		this.newTask = "";
	  },
	  deleteTask(taskId) {
		const index = this.tasks.findIndex((task) => task.id === taskId);
		if (index !== -1) {
		  this.tasks.splice(index, 1);
		}
	  },
	  markCompleted(task) {
		task.completed = !task.completed;
	  },
	},
  };
  </script>