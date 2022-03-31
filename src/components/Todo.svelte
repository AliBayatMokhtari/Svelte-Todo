<script>
  import { createEventDispatcher } from "svelte";

  export let title = "";
  export let checked = false;
  export let id = "";
  const dispatch = createEventDispatcher();

  const submitForm = (e) => {
    e.preventDefault();
    dispatch("submit", id ? { title, checked, id } : { title, checked });
  };
</script>

<form on:submit={submitForm} class="todo">
  <input placeholder="Todo title" id="title" on:input={(e) => (title = e.target.value)} value={title || ""} />
  <input type="submit" />
</form>

<style>
  .todo {
    margin-top: 30px;
  }
</style>
