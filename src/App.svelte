<script>
  import { afterUpdate } from "svelte";

  let todoItems = [];
  let newTodo = "";

  const addTodo = () => {
    newTodo = newTodo.trim();

    if (!newTodo) {
      return;
    }

    const todo = {
      text: newTodo,
      checked: false,
      id: Date.now(),
    };

    todoItems = [...todoItems, todo];
    newTodo = "";
  };

  const toggleDone = (id) => {
    const index = todoItems.findIndex(({ id: _id }) => _id === Number(id));
    todoItems[index].checked ^= 1;
  };

  const deleteTodo = (id) => {
    todoItems = todoItems.filter(({ id: _id }) => _id !== Number(id));
  };

  afterUpdate(() => {
    document.querySelector(".js-todo-input").focus();
  });
</script>

<main>
  <div class="container">
    <h1 class="app-title">todos</h1>
    <ul class="todo-list" />
    <div class="empty-state">
      <svg class="checklist-icon"><use href="#checklist-icon" /></svg>
      <h2 class="empty-state__title">Add your first todo</h2>
      <p class="empty-state__description">
        What do you want to get done today?
      </p>
    </div>
    <form on:submit|preventDefault={addTodo}>
      <input
        class="js-todo-input"
        type="text"
        aria-label="Enter a new todo item"
        placeholder="E.g. Build a web API"
        bind:value={newTodo} />
    </form>
  </div>

  <ul class="todo-list">
    {#each todoItems as todo (todo.id)}
      <li class="todo-item {todo.checked ? 'done' : ''}">
        <input id={todo.id} type="checkbox" />
        <label
          for={todo.id}
          class="tick"
          on:click={() => toggleDone(todo.id)} />
        <span>{todo.text}</span>
        <button class="delete-todo" on:click={() => deleteTodo(todo.id)}>
          <svg><use href="#delete-icon" /></svg>
        </button>
      </li>
    {/each}
  </ul>
</main>
