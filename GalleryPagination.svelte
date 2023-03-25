<script>
  import { createEventDispatcher } from 'svelte';

  export let images = [];
  export let currentSlide;
  export let animationTime = 10000;

  const dispatch = createEventDispatcher();

  const clickHandler = (event) => {
    let targetSlideId = event.target.dataset.index;
    dispatch('slideChange', {targetSlideId});
  };
</script>

<ul class="pagination" style="--animation-time: {animationTime}ms">
  {#each images as image, i}
    <li
        class="pagination__item"
        class:active={currentSlide === i}
        data-index={i}
        on:click={clickHandler}
    >
      {i + 1}
    </li>
  {/each}
</ul>

<style>
    .pagination {
        bottom: var(--skbs-pagination-offset-bottom, 56px);
        display: flex;
        gap: 4px;
        justify-content: space-between;
        left: 0;
        list-style: none;
        margin: 0;
        padding: 0 3px;
        position: absolute;
        right: 0;
        z-index: 23;
    }

    .pagination__item {
        background-color: rgba(255, 255, 255, 0.35);
        border-radius: 5px;
        color: transparent;
        cursor: pointer;
        display: block;
        flex: 1 0.5 10em;
        height: 6px;
        list-style: none;
        margin: 0;
        padding: 0 2px;
        position: relative;
        text-align: center;
        transition: flex-grow 500ms ease;
        width: auto;
        will-change: flex-grow;
    }

    .pagination__item.active {
        background-color: rgba(255, 255, 255, 0.65);
        flex-grow: 1.5;
        height: 6px;
    }

    .pagination__item.active::before {
        animation-duration: var(--animation-time);
        animation-name: running-progress;
        animation-timing-function: linear;
        background-color: rgba(var(--skbs-progress-bar-color, 255, 0, 255, 0.5));
        border-radius: 5px;
        bottom: 0;
        content: '';
        height: 6px;
        left: 0;
        overflow: hidden;
        position: absolute;
        top: 0;
        width: 0;
    }

    @keyframes -global-running-progress {
        from {
            width: 0;
        }

        to {
            width: 100%;
        }
    }
</style>
