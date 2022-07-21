<script lang="ts">
  import Board from "./components/Board.svelte";
  export let name: string;
  let size = 5;
</script>

<main>
  <h1>Hello {name}!</h1>
  <Board bind:size />
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 100%;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }
</style>
