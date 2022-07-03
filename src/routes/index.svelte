<script>
  let todos = [ ]

  let task = ''
  
  const addTodo = () => {
    if(task.trim() != '') {
      let todo = {
        task: task.trim(),
        isComplete : false,
        createdAt: new Date(),
      }
      todos = [...todos, todo];
    }
    task = '';
  }

  const keyIsPressed = (e) => {
    if(e.key === 'Enter') {
      addTodo();
    }
  }
  
  const markTodoAsComplete = (index) => {
    // console.log(index)
    todos[index].isComplete = !todos[index].isComplete;
  }

  const deleteTodo = (index) => {
    let deleteItem = todos[index];
    todos = todos.filter((item) => item != deleteItem)
  }
  
  $:console.table(todos)
</script>

<input type='text' placeholder="Add a task" bind:value={task} />
<button on:click={addTodo} on:keydown={handleEnter} >Add</button>

<ol>
  {#each todos as item, index}
    <li class:complete={item.isComplete}>
      <span>
        {item.task} at {item.createdAt}
      </span>
      <span>
        <button on:click={()=>markTodoAsComplete(index)}>✅</button>
        <button on:click={()=>deleteTodo(index)}>❎</button>
      </span>
    </li>
  {:else}
    <p>No todo found</p>
  {/each}
</ol>

<svelte:window on:keydown={keyIsPressed} />

<style>
  .complete {
    text-decoration: line-through;
  }
</style>