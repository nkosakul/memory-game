<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  const countOptions = [6, 8, 10, 12, 14, 16, 18, 20];
  let selected;

  const handleSubmit = () => {
    dispatch('submit', {
      cards: selected,
    });
  };
</script>

<div class="intro">
  <div class="intro__inner">
    <h1>Memory Game!</h1>
    <p>Hey there! Ready to play some good old Memory, like in the childhood?</p>
    <form action="/" on:submit|preventDefault={handleSubmit}>
      <label for="count">How many Cards?</label>
      <div class="select-field">
        <select name="count" id="count" bind:value={selected}>
          {#each countOptions as option}
            <option value={option}>{option}</option>
          {/each}
        </select>
        <span class="arrow" aria-hidden="true" />
      </div>

      <button class="submit">Let´s do this!</button>
    </form>
  </div>
</div>

<style type="text/scss">
  .intro {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .intro__inner {
    background-color: white;
    max-width: 800px;
    width: 100%;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 10px 15px -10px rgba(0, 0, 0, 0.7);
  }

  form {
    text-align: left;
    margin-top: 50px;
  }

  .select-field {
    position: relative;
    margin-bottom: 20px;
  }

  label {
    margin-bottom: 10px;
  }

  select {
    width: 100%;
    border: 1px solid #ddd;
    padding: 15px 15px;
  }

  .arrow {
    position: absolute;
    top: 50%;
    right: 20px;
    border: solid #222;
    border-width: 0 3px 3px 0;
    display: inline-block;
    padding: 3px;
    transform: translateY(-50%) rotate(45deg);
  }

  button {
    width: 100%;
    background-color: #2980b9;
    padding: 15px;
    border-radius: 3px;
    color: white;
    font-size: 20px;
    transition: background-color 300ms ease-in-out;

    &:hover,
    &:focus {
      background-color: #49a0d9;
    }
  }
</style>
