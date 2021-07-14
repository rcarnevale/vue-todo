<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TaskProgress :progress="progress"></TaskProgress>
		<NewTask @taskAdded="addTask($event)"></NewTask>
		<TaskGrid :tasks="tasks"
		@taskDeleted="deleteTask"
		@taskStateChanged="toggleTaskStatus"></TaskGrid>
	</div>
</template>

<script>
import TaskGrid from './components/TaskGrid.vue';
import NewTask from './components/NewTask.vue';
import TaskProgress  from './components/TaskProgress.vue';

export default {
	components: { TaskGrid, NewTask, TaskProgress },
	data() {
		return {
			tasks: []
		}
	},
	created() {
		const json = localStorage.getItem('tasks');
		this.tasks = JSON.parse(json) || [];
	},
	watch: {
		tasks: {
			deep: true,
			handler() {
				localStorage.setItem('tasks', JSON.stringify(this.tasks));
			}
		}
	},
	methods: {
		addTask(task) {
			const reallyNew = this.tasks.filter(t => t.name === task.name).length == 0;
			if(reallyNew){
				const newTask = { name: task.name, done: false };
				this.tasks.push(newTask);
			}
		},
		deleteTask(id) {
			this.tasks.splice(id, 1);
		},
		toggleTaskStatus(id){
			this.tasks[id].done = !this.tasks[id].done;
		}
	},
	computed: {
		progress() {
			const total = this.tasks.length;
			const done = this.tasks.filter(t => t.done).length;
			return Math.round(done / total * 100) || 0;
		}
	}
	

}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
