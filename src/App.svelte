<script>
  import { onMount } from 'svelte';
  import Head from './components/Head.svelte';
  import Cards from './components/Cards.svelte';

  let finishedCards = [];
  let cardLimit = 12;

  const handleCard = event =>
    (finishedCards = [...finishedCards, event.detail.photo]);
</script>

<div class="container">
  <Head {finishedCards} {cardLimit} />
  <Cards on:addCard={handleCard} {cardLimit} />
</div>

<style type="text/scss">
  .container {
    max-width: 1440px;
    width: 100%;
    margin: 0 auto;
    padding: 10px;
  }
</style>
