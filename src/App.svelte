<script>
  const todos = [];

  function submit() {
    console.log("form submitted");
  }
</script>

<main class="container">
  <div class="card">
    <form>
      <!-- <form on:submit={submit}> -->
      <div class="u-flex u-gap-12">
        <input
          type="text"
          class="input-text u-text-start"
          placeholder="Enter todo"
        />
        <button class="button" on:click={submit}>
          <span class="icon-plus" />
          <span class="text">Add</span></button
        >
      </div>
    </form>

    <ul class="list u-text-start u-margin-block-start-32">
      <li>
        <input type="checkbox" id="" class="input-check" />
        <label for="" />
      </li>
    </ul>
  </div>
</main>

<style>
  .container {
    max-width: 35rem;
  }
</style>
