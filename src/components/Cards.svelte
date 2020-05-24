<script>
  import { onMount, createEventDispatcher } from 'svelte';
  import { fly } from 'svelte/transition';

  const dispatch = createEventDispatcher();
  export let cardLimit;
  let photos = [];
  let activeCards = [];

  const handleClick = (e, photo) => {
    const button = e.currentTarget;

    // return, if user clicked on the active button again
    if (button.classList.contains('is-active') || activeCards.length === 2)
      return;

    button.classList.add('is-active');
    activeCards.push(photo.id);

    if (activeCards.length === 2) {
      const isEqual = activeCards.every(id => id === activeCards[0]);
      const cards = [...document.querySelectorAll(`.card-${photo.id}`)];

      if (isEqual) {
        dispatch('addCard', {
          photo,
        });

        activeCards.forEach(id => {
          const cards = [...document.querySelectorAll(`.card-${id}.is-active`)];
          setTimeout(() => {
            cards.forEach(card => card.classList.add('is-finished'));
          }, 800); // wait for flip animation to end
        });

        activeCards = [];
      } else {
        activeCards.forEach(id => {
          const card = document.querySelector(`.card-${id}.is-active`);

          setTimeout(() => {
            card.classList.add('is-shaking');
          }, 800); // wait for flip animation to end

          setTimeout(() => {
            card.classList.remove('is-active');
            card.classList.remove('is-shaking');
            activeCards = [];
          }, 2000); // give user time to memorize the images
        });
      }
    }
  };

  onMount(async () => {
    const res = await fetch(
      `https://picsum.photos/v2/list?page=2&limit=${cardLimit}`
    );
    let response = await res.json();
    console.log(response);
    let arr = await response.concat(response); // duplicate the items
    photos = arr.sort(() => Math.random() - 0.5); // shuffle the array
  });
</script>

<main>
  <div class="photos">
    {#each photos as photo}
      <button
        class={`card card-${photo.id}`}
        on:click={e => handleClick(e, photo)}
        transition:fly={{ y: 10, duration: 200 }}>
        <div class="card__inner">
          <div class="card__front">
            <img src={photo.download_url} alt={photo.author} />
          </div>
          <div class="card__back" />
        </div>
      </button>
    {:else}
      <p class="loading">loading cards...</p>
    {/each}
  </div>
</main>

<style type="text/scss">
  @mixin fixed-ratio($ratio: 16/9, $selector: '> *') {
    $selector: unquote($selector);

    position: relative;

    &::after {
      content: '';
      display: block;
      height: 0;
      padding-bottom: (1 / $ratio) * 100%;
    }

    #{$selector} {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    }
  }

  .photos {
    width: 100%;
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    grid-gap: 8px;
  }

  img {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    object-fit: cover;
  }

  .card {
    position: relative;
    overflow: hidden;
    display: block;
    width: 100%;
    background-color: white;
    transition: transform 250ms ease-in-out;

    :global(&:not(.is-active) .card__inner) {
      transform: rotateY(180deg);
    }

    :global(&.is-finished) {
      animation: finished 550ms forwards;

      @keyframes finished {
        0% {
          transform: scale(1);
        }

        50% {
          transform: scale(1.05);
          z-index: 1;
        }

        70% {
          transform: scale(1);
          opacity: 1;
        }

        100% {
          z-index: 0;
          opacity: 0.4;
        }
      }
    }

    :global(&.is-shaking) {
      animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;

      @keyframes shake {
        10%,
        90% {
          transform: translate3d(-1px, 0, 0);
        }

        20%,
        80% {
          transform: translate3d(2px, 0, 0);
        }

        30%,
        50%,
        70% {
          transform: translate3d(-4px, 0, 0);
        }

        40%,
        60% {
          transform: translate3d(4px, 0, 0);
        }
      }
    }
  }

  .card__inner {
    @include fixed-ratio(1/1);

    position: relative;
    perspective: 1000px;
    transition: transform 0.6s;
    transform-style: preserve-3d;
  }

  .card__front,
  .card__back {
    position: absolute;
    width: 100%;
    height: 100%;
    transform-style: preserve-3d;
    backface-visibility: hidden;
  }

  .card__back {
    background-color: #2980b9;
    transform: rotateY(180deg);
  }
</style>
