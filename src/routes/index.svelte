<script>
  let todos = [ ]

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

  const markTodoAsComplete = (index) => {
    // console.log(index)
    todos[index].isComplete = !todos[index].isComplete;
  }
  
  $:console.table(todos)
</script>

<input type='text' placeholder="Add a task" bind:value={task} />
<button on:click={addTodo} >Add</button>

<ol>
  {#each todos as item, index}
    <li class:complete={item.isComplete}>
      <span>
        {item.task} at {item.createdAt}
      </span>
      <span>
        <button on:click={()=>markTodoAsComplete(index)}>✔</button>
      </span>
    </li>
  {/each}
</ol>

<style>
  .complete {
    text-decoration: line-through;
  }
</style>