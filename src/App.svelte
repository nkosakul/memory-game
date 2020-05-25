<script>
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

  const handleReset = () => {
    finishedCards = 0;
  };

  const handleCard = event => finishedCards++;
</script>

<div class="container">
  {#if gameStarted}
    <Head on:reset={handleReset} {finishedCards} {cardLimit} />
    <Cards on:addCard={handleCard} {cardLimit} />
  {:else}
    <Intro on:submit={handleSubmit} />
  {/if}
</div>
<footer>
  <p>
    <small>by nkosakul</small>
  </p>
  <a href="https://github.com/nkosakul/memory-game">Github</a>
</footer>

<style type="text/scss">
  .container {
    max-width: 1440px;
    width: 100%;
    margin: 0 auto;
    padding: 10px;
  }

  footer {
    display: flex;
    justify-content: space-between;
    padding: 10px;

    p {
      margin: 0;
    }
  }
</style>
