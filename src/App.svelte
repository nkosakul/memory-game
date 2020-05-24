<script>
  import { onMount } from 'svelte';
  import Head from './components/Head.svelte';
  import Cards from './components/Cards.svelte';
  import Intro from './components/Intro.svelte';

  let finishedCards = 0;
  let cardLimit = 6;
  let gameStarted = false;

  const handleSubmit = event => {
    cardLimit = event.detail.cards;
    gameStarted = true;
  };

  const handleCard = event =>
    (finishedCards = [...finishedCards, event.detail.photo]);
</script>

<div class="container">
  {#if gameStarted}
    <Head {finishedCards} {cardLimit} />
    <Cards on:addCard={handleCard} {cardLimit} />
  {:else}
    <Intro on:submit={handleSubmit} />
  {/if}
</div>

<style type="text/scss">
  .container {
    max-width: 1440px;
    width: 100%;
    margin: 0 auto;
    padding: 10px;
  }
</style>
