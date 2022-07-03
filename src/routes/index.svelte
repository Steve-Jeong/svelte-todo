<script>
  let todos = [
    {task:'Get milk', isComplete:false, createdAt: new Date()},
    {task:'Get eggs', isComplete:true, createdAt: new Date()},
    {task:'Get bread', isComplete:false, createdAt: new Date()},
  ]

  let task = ''
  
  const addTodo = () => {
    let todo = {
      task: task,
      isComplete : false,
      createdAt: new Date(),
    }
    todos = [...todos, todo];
    task = '';
  }

  $:console.table(todos)
</script>

<input type='text' placeholder="Add a task" bind:value={task} />
<button on:click={addTodo} >Add</button>

<ol>
  {#each todos as todo}
    <li class:complete={todo.isComplete}>{todo.task} at {todo.createdAt}</li>
  {/each}
</ol>

<style>
  .complete {
    text-decoration: line-through;
  }
</style>