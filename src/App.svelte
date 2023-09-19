<script>
  import Actions from "./lib/Actions.svelte";
  import Output from "./lib/Output.svelte";
</script>

<main>
  <section class="content">
    <Output />
  </section>
  <section class="actions">
    <Actions />
  </section>
</main>

<style lang="scss">
  main {
    display: flex;
    margin: 2rem 3rem 0 3rem;
    justify-content: space-between;
  }

  .content {
    width: 67%;
  }

  .actions {
    border-left: 1px dotted #bfa38a;
    padding-left: 2rem;
  }
</style>
