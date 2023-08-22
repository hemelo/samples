<script>
  import { useTracker } from 'meteor/rdb:svelte-meteor-data';
  import TaskForm from './TaskForm.svelte';
  import Task from './Task.svelte';
  import { TasksCollection } from '../api/TasksCollection';

  $: tasks = useTracker(() => TasksCollection.find({}, { sort: { createdAt: -1 }}).fetch());
</script>


<div class="app">
  <header>
      <div class="app-bar">
          <div class="app-header">
              <h1>📝️ To Do List</h1>
          </div>
      </div>
  </header>

  <div class="main">
      <TaskForm />

      <ul class="tasks">
          {#each tasks as task (task._id)}
              <Task task={task} />
          {/each}
      </ul>
  </div>
</div>