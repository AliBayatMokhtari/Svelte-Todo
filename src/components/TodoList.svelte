<script>
  import { generateUUID } from "../utils/uuid";
  import Todo from "../components/Todo.svelte";
  let todos = [];
  let todoToEdit = {};

  const addTodo = (todo) => {
    const newTodo = { ...todo, id: generateUUID() };
    todos = [...todos, newTodo];
  };

  const removeTodo = (id) => {
    todos = [...todos.filter((todo) => todo.id !== id)];
    if (todoToEdit.id === id) todoToEdit = {};
  };

  const updateTodo = (todo) => {
    todos = [...todos.map((t) => (t.id !== todo.id ? t : { ...t, title: todo.title, checked: todo.checked }))];
  };

  const toggleChecked = (id) => {
    todos = [...todos.map((todo) => (todo.id !== id ? todo : { ...todo, checked: !todo.checked }))];
  };
</script>

<div class="center">
  <div class="heading-1">TODO Tracker</div>
  <Todo
    on:submit={(data) => (data.detail.id ? updateTodo(data.detail) : addTodo(data.detail))}
    title={todoToEdit.title}
    checked={todoToEdit.checked}
    id={todoToEdit.id}
  />
  <ul class="todo_list">
    {#if !todos.length}
      <div>NO TODOs...</div>
    {:else}
      {#each todos as todo}
        <li class="todo_list_item">
          {#if todo.checked}
            <div style="margin-right: 5px; font-size: 18px; margin-top: 2px;">✅</div>
          {/if}
          <span on:click={toggleChecked(todo.id)} class={`todo_title ${todo.checked ? "done" : null}`}>
            {todo.title}
          </span>
          <div on:click={() => (todoToEdit = { ...todo })} class="removeIcon">✍️</div>
          <div on:click={removeTodo(todo.id)} class="removeIcon">❌</div>
        </li>
      {/each}
    {/if}
  </ul>
</div>

<style>
  .center {
    flex: 1;
    display: flex;
    align-items: center;
    flex-direction: column;
  }
  .heading-1 {
    font-size: 32px;
    font-weight: 800;
    color: rgba(0, 0, 0, 0.7);
  }

  .todo_title {
    cursor: pointer;
  }

  .todo_list {
    width: 500px;
    margin-top: 30px;
  }

  .todo_list_item {
    list-style: none;
    display: flex;
    align-items: center;
    height: 30px;
  }

  .done {
    text-decoration: line-through;
  }

  .removeIcon {
    cursor: pointer;
    font-size: 16px;
    margin-left: 10px;
    margin-top: 2px;
  }
</style>
