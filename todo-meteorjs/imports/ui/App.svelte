<script>
  import { useTracker } from 'meteor/rdb:svelte-meteor-data';
  import TaskForm from './TaskForm.svelte';
  import Task from './Task.svelte';
  import { TasksCollection } from '../api/TasksCollection';

  $: tasks = useTracker(() => TasksCollection.find({}, { sort: { createdAt: -1 }}).fetch());
</script>


<div class="container">
  <header>
    <h1>Todo List</h1>
  </header>

  <TaskForm />

  <ul>
    {#each tasks as task (task._id)}
        <Task task={task} />
    {/each}
  </ul>
</div>