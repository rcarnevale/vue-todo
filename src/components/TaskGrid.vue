<template>
  <div class="tasks-grid">
      <template v-if="tasks.length">
          <Task v-for="(task, id) of tasks" 
          :key="task.name" :task="task"
          @taskDeleted="$emit('taskDeleted', id)"
          @taskStateChanged="$emit('taskStateChanged', id)"></Task> 
      </template>
      <p class="no-task" v-else>Sua vida está em dia. :)</p>
  </div>
</template>

<script>
import Task from './Task.vue';

export default {
    components: { Task },
    props: {
        tasks: {type: Array, required: true }
    }
}
</script>

<style>

.tasks-grid {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
}

.tasks-grid .task {
    margin: 10px;
}

.no-task {
    color: #aaa;
    font-size: 1.7rem;
}

</style>