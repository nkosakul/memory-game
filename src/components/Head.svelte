<script>
  export let finishedCards;
  export let cardLimit;
</script>

<header class="head">
  <h1>Memory Game</h1>

  <p>{finishedCards} / {cardLimit}</p>
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
</style>
