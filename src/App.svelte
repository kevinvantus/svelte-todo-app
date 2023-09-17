<script>
  // @ts-nocheck
  let todoTitle = "";
  $: todos = [];
  let todoExists = false;
  const priorities = {
    low: "low",
    normal: "normal",
    high: "high",
  };

  function submit() {
    if (!todoTitle.length) return;

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

  function toggleTodo(id) {
    todos = todos.map(todo => {
      if (todo.id === id) return { ...todo, done: true };
      return todo;
    });
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
      <ul class="list u-gap-16 u-text-start u-margin-block-start-24">
        {#each todos as todo (todo.id)}
          <li class="u-flex u-cross-center u-gap-8">
            <input
              type="checkbox"
              id={todo.title}
              class="input-check"
              bind:checked={todo.done}
              on:change={toggleTodo(todo.id)}
            />

            <label for={todo.title} class="u-cursor-pointer u-capitalize"
              >{todo.title}</label
            >

            <button
              type="button"
              class="button u-margin-inline-start-auto u-color-text-info is-text is-only-icon u-padding-0"
            >
              <span class="icon-pencil" />
            </button>
            <button
              type="button"
              class="button u-color-text-danger is-text is-only-icon u-padding-0"
            >
              <span class="icon-trash" />
            </button>
          </li>
        {/each}
      </ul>
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
