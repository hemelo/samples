<script>
  import { useTracker } from 'meteor/rdb:svelte-meteor-data';
  import TaskForm from './TaskForm.svelte';
  import Task from './Task.svelte';
  import { TasksCollection } from '../api/TasksCollection';

  let hideCompleted = false;
    
  const setHideCompleted = value => hideCompleted = value;

  let incompleteCount;
  let pendingTasksTitle = '';
  let tasks = [];

  $: { 
    tasks = useTracker(() =>  TasksCollection.find(
      hideCompleted ? hideCompletedFilter : {}, { sort: { createdAt: -1 } }
    ).fetch());

    incompleteCount = TasksCollection.find(hideCompletedFilter).count();

    pendingTasksTitle = `${
            incompleteCount ? ` (${incompleteCount})` : ''
    }`;
  }

  const hideCompletedFilter = { isChecked: { $ne: true } };

</script>


<div class="app">
  <header>
      <div class="app-bar">
          <div class="app-header">
              <h1>📝️ To Do List {pendingTasksTitle}</h1>
          </div>
      </div>
  </header>

  <div class="main">
      <TaskForm />

      <div class="filter">
        <button on:click={() => setHideCompleted(!hideCompleted)}>
            {hideCompleted ? 'Show All' : 'Hide Completed'}
        </button>
      </div>

      <ul class="tasks">
          {#each tasks as task (task._id)}
              <Task task={task} />
          {/each}
      </ul>
  </div>
</div>