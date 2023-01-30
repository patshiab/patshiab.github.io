<!-- using tutorial from https://freshman.tech/svelte-todo/-->

<script>
  import { afterUpdate } from 'svelte';
    import { element } from 'svelte/internal';

  afterUpdate(() => {
    document.querySelector('.js-todo-input').focus();
  });

  let todoItems = [];
  let newItem = '';   

  function addTodo() {
    newItem = newItem.trim();
    if (!newItem) return;   /* #controlFlow */

    const todo = {
        text: newItem,
        checked: false,
        id: Date.now(),
    };


    todoItems = [...todoItems, todo];
    newItem = '';     /* #Reactive */
  }

  function finishedItem(id) {
    const index = todoItems.findIndex(item => item.id === Number(id));
    todoItems[index].checked = !todoItems[index].checked;
  }

  function deleteTodo(id) {
    todoItems = todoItems.filter(item => item.id !== Number(id));
  }

  function clearTodo() {      
    todoItems.forEach(function(element){      
      if (element.checked == true){     /* #controlFlow */
        deleteTodo(element.id)
      }
    })
  }

</script>

<main>
  <div class="container">
    <h1 class="app-title">todos</h1>    <!-- #component -->
    <ul class="todo-list"></ul>     <!-- #component -->
    <div class="empty-state">
      <svg class="checklist-icon"><use href="#checklist-icon"></use></svg>
      <h2 class="empty-state__title">Add your first todo</h2>     <!-- #component -->
      <p class="empty-state__description">What do you need to finish today?</p>     <!-- #component -->
    </div>
  <ul class="todo-list">
    {#each todoItems as todo (todo.id)}     <!-- #controlFlow, #Reactive-->
      <li class="todo-item {todo.checked ? 'done' : ''}">
        <input id={todo.id} type="checkbox" />
        <label for={todo.id} class="tick" on:click={() => finishedItem(todo.id)}></label>
        <span>{todo.text}</span>
        <button class="delete-todo" on:click={() => deleteTodo(todo.id)}>
          <svg><use href="#delete-icon"></use></svg>
        </button>
      </li>
    {/each}
  </ul>
  <form on:submit|preventDefault={addTodo}>
    <input class="js-todo-input" type="text" aria-label="Enter a new todo item" placeholder="E.g. Finish CS 178 Homework" bind:value={newItem}>     <!-- #Reactive --> 
  </form>
  <button class="clear list" on:click={() => clearTodo(todoItems)}>     <!-- #Reactive -->
    clear completed items
  </button>
  </div>
</main>