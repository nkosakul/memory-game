<script>
  import { createEventDispatcher } from 'svelte';
  import { resets } from '../stores/store.js';

  export let finishedCards;
  export let cardLimit;

  const dispatch = createEventDispatcher();

  const handleClick = () => {
    dispatch('reset');
    resets.update(n => n + 1);
  };
</script>

<header class="head">
  <h1>Memory Game</h1>

  <div class="head__meta">
    <p>{finishedCards} / {cardLimit}</p>
    <button class="reset-btn" on:click={handleClick}>Reset Game</button>
  </div>
</header>

<style type="text/scss">
  .head {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
  }

  h1 {
    margin: 0;
  }

  p {
    margin: 0;
  }

  .head__meta {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    flex: auto;
    padding-left: 20px;
  }

  .reset-btn {
    margin-left: 20px;
    background-color: #ddd;
    padding: 10px 15px;
    border-radius: 30px;
    color: #666;
    transition: all 300ms ease-in-out;

    &:hover {
      color: white;
      background-color: #2980b9;
    }
  }
</style>
