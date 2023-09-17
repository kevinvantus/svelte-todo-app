<script>
  import TodoList from "./lib/TodoList.svelte";

  // @ts-nocheck
  let todoTitle = "";
  $: todos = [];
  let todoExists = false;
  let editing = false;
  let editIndex = null;
  const priorities = {
    low: "low",
    normal: "normal",
    high: "high",
  };

  function submit() {
    if (!todoTitle.length) return;

    if (editing) {
      todos[editIndex].title = todoTitle;
      todoTitle = "";
      editing = false;
      return;
    }

    const todo = {
      id: crypto.randomUUID(),
      title: todoTitle.trim(),
      done: false,
      priority: "normal",
    };
    const todoIndex = todos.findIndex(
      todo => todo.title.toLowerCase() === todoTitle.toLowerCase()
    );

    if (todoIndex === -1) {
      todos = [...todos, todo];
      todoTitle = "";
    }
  }

  function input() {
    const todoIndex = todos.findIndex(
      todo => todo.title.trim().toLowerCase() === todoTitle.trim().toLowerCase()
    );

    if (todoIndex > -1) {
      todoExists = true;
    } else {
      todoExists = false;
    }
  }

  function toggleTodo(evt) {
    const { id, done } = evt.detail;

    todos = todos.map(todo => {
      if (todo.id === id) return { ...todo, done };
      return todo;
    });
  }

  function editTodo(evt) {
    const { id, idx } = evt.detail;
    todoTitle = todos.find(todo => todo.id === id).title;
    editIndex = idx;
    editing = true;
  }

  function deleteTodo(evt) {
    todos = todos.filter(todo => todo.id !== evt.detail.id);
  }
</script>

<main class="container">
  <h1 class="heading-level-1 u-text-center">Todo List</h1>

  <div class="card u-margin-block-start-24">
    <form on:submit|preventDefault={submit}>
      <div class="u-flex u-gap-12">
        <input
          type="text"
          class="input-text u-text-start"
          placeholder="Enter todo"
          bind:value={todoTitle}
          on:input={input}
        />
        <button
          class="button"
          style="background-color: hsl(var(--color-information-100)); border-color: hsl(var(--color-information-100));"
          on:click={submit}
        >
          <span class="icon-plus" />
          <span class="text">Add</span></button
        >
      </div>
    </form>

    {#if todoExists}
      <div class="u-text-center u-color-text-warning u-margin-block-start-16">
        <p class="u-flex u-cross-center u-main-center">
          <span class="icon-exclamation" />
          <span>{todoTitle} already exists.</span>
        </p>
      </div>
    {/if}

    {#if todos.length}
      <TodoList
        {todos}
        on:toggle={toggleTodo}
        on:edit={editTodo}
        on:delete={deleteTodo}
      />
    {:else}
      <p class="u-margin-block-start-24 u-text-center">
        You have not added any todo(s).
      </p>
    {/if}
  </div>
</main>

<style>
  .container {
    max-width: 35rem;
  }
</style>
