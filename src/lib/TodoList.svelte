<script>
  import { createEventDispatcher } from "svelte";

  export let todos = [];
  const dispatch = createEventDispatcher();
</script>

<ul class="list u-gap-16 u-text-start u-margin-block-start-24">
  {#each todos as todo, i (todo.id)}
    <li class="u-flex u-cross-center u-gap-8">
      <input
        type="checkbox"
        id={todo.title}
        class="input-check"
        bind:checked={todo.done}
        on:change={() => dispatch("toggle", { idx: i, done: todo.done })}
      />

      <label for={todo.title} class="u-cursor-pointer u-capitalize"
        >{todo.title}</label
      >

      <button
        type="button"
        class="button u-margin-inline-start-auto u-color-text-info is-text is-only-icon u-padding-0"
        on:click={() => dispatch("edit", { id: todo.id, idx: i })}
      >
        <span class="icon-pencil" />
      </button>
      <button
        type="button"
        class="button u-color-text-danger is-text is-only-icon u-padding-0"
        on:click={() => dispatch("delete", { id: todo.id })}
      >
        <span class="icon-trash" />
      </button>
    </li>
  {/each}
</ul>
