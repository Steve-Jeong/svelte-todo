<script>
  import { initializeApp, getApps, getApp } from "firebase/app";
  import { getFirestore, collection, onSnapshot, doc, updateDoc, deleteDoc } from "firebase/firestore";
  import { firebaseConfig } from "$lib/firebaseConfig";

  // Initialize Firebase
  const firebaseApp = 
    getApps().length === 0 ? 
      initializeApp(firebaseConfig) : 
      getApp()

  // Initialize Cloud Firestore and get a reference to the service
  const db = getFirestore(firebaseApp);

  // console.log(firebaseApp);
  // console.log(db)

  // collection
  const colRef = collection(db, 'todos');
  // console.log(colRef)

  let todos = [];
  const unsubscribe = onSnapshot(colRef, (querySnapshot) => {
    let fbTodos = []
    querySnapshot.forEach((doc) => {
      let todo = {...doc.data(), id: doc.id}
      fbTodos = [todo, ...fbTodos];
    });
    console.table('todos', fbTodos);
    todos = fbTodos;
  });

  let task = "";
  let error = "";

  const addTodo = () => {
    if (task.trim() != "") {
      let todo = {
        task: task.trim(),
        isComplete: false,
        createdAt: new Date(),
      };
      todos = [...todos, todo];
      error = "";
    } else {
      error = "task is empty";
    }
    task = "";
  };

  const keyIsPressed = (e) => {
    if (e.key === "Enter") {
      addTodo();
    }
  };

  const markTodoAsComplete = async (item) => {
    // console.log(index)
    // todos[index].isComplete = !todos[index].isComplete;
    await updateDoc(doc(db, 'todos', item.id), {
      isComplete : !item.isComplete,
    })
  };

  const deleteTodo = async (id) => {
    // let deleteItem = todos[index];
    // todos = todos.filter((item) => item != deleteItem);
    await deleteDoc(doc(db, 'todos', id))
  };

  $: console.table(todos);
</script>

<input type="text" placeholder="Add a task" bind:value={task} />
<button on:click={addTodo}>Add</button>

<ol>
  {#each todos as item(item.id)}
    <li class:complete={item.isComplete}>
      <span>
        {item.task} at {item.createdAt}
      </span>
      <span>
        <button on:click={() => markTodoAsComplete(item)}>✅</button>
        <button on:click={() => deleteTodo(item.id)}>❎</button>
      </span>
    </li>
  {:else}
    <p>No todo found</p>
  {/each}
  <p class="error">{error}</p>
</ol>

<svelte:window on:keydown={keyIsPressed} />

<style>
  .complete {
    text-decoration: line-through;
  }
  .error {
    color: red;
  }
</style>
