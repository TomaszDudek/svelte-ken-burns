<script>
  import { onDestroy, onMount } from 'svelte';
  import GalleryPagination from './GalleryPagination.svelte';

  export let images = [];
  export let fadeDuration = 5000;
  export let slideDuration = 25000;
  export let showArrows = true;
  export let showPagination = true;

  const animationNames = [
    'ken-burns-center',
    'ken-burns-top-right',
    'ken-burns-top-left',
    'ken-burns-top-middle',
    'ken-burns-bottom-right',
    'ken-burns-bottom-left',
    'ken-burns-bottom-middle',
    'ken-burns-middle-left',
    'ken-burns-middle-right'
  ];

  let imagePool;
  let slideDurationTimeOut = 0;
  let replaceTimeOut = 0;
  let currentSlide;
  const chevron = `<svg class="icon" xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="298" height="512" shape-rendering="geometricPrecision" text-rendering="geometricPrecision" image-rendering="optimizeQuality" fill-rule="evenodd" clip-rule="evenodd" viewBox="0 0 298 511.93"><path fill-rule="nonzero" d="M70.77 499.85c-16.24 16.17-42.53 16.09-58.69-.15-16.17-16.25-16.09-42.54.15-58.7l185.5-185.03L12.23 70.93c-16.24-16.16-16.32-42.45-.15-58.7 16.16-16.24 42.45-16.32 58.69-.15l215.15 214.61c16.17 16.25 16.09 42.54-.15 58.7l-215 214.46z"/></svg>`;

  onMount(() => {
    initSlideShow();
  });

  onDestroy(() => {
    stopTimeOuts();
  });

  const stringToBoolean = (value) => {
    return String(value) === '1' || String(value).toLowerCase() === 'true';
  };

  const initSlideShow = (startAt = 0) => {
    if (!images.length) {
      return;
    }

    currentSlide = startAt;
    imagePool.replaceChildren();
    const img = document.createElement('img');
    img.classList.add('kenburns-image');
    img.src = images[startAt];
    img.onload = () => {
      stopTimeOuts();
      replaceImage(startAt, img);
    };
  };

  const stopTimeOuts = () => {
    clearTimeout(slideDurationTimeOut);
    clearTimeout(replaceTimeOut);
  };

  const replaceImage = (index, img) => {
    const nextIndex = (index + 1) % images.length;
    const rand = Math.random();
    const slideDirection = rand > 0.5 ? 'normal' : 'reverse';
    const animationIndex = Math.floor(rand * animationNames.length);
    currentSlide = index;

    const imageWrapper = document.createElement('div');
    imageWrapper.classList.add('image-wrapper');
    imageWrapper.appendChild(img);
    imageWrapper.style.animationDirection = `${slideDirection}, normal`;
    imageWrapper.style.animationName = `${animationNames[animationIndex]}, fade-in`;

    imagePool.appendChild(imageWrapper);

    replaceTimeOut = setTimeout(() => {
      imageWrapper.remove();
    }, slideDuration);

    const nextImage = document.createElement('img');
    nextImage.classList.add('kenburns-image');
    nextImage.src = images[nextIndex];

    slideDurationTimeOut = setTimeout(() => {
      replaceImage(nextIndex, nextImage);
    }, slideDuration - fadeDuration);
  };

  const next = () => {
    if (currentSlide < images.length - 1) {
      initSlideShow(currentSlide + 1);
      return;
    }
    initSlideShow(0);
  };

  const prev = () => {
    if (currentSlide !== 0) {
      initSlideShow(currentSlide - 1);
      return;
    }
    initSlideShow(images.length - 1);
  };

  const handleSlideChange = (event) => {
    initSlideShow(parseInt(event.detail.targetSlideId));
  };

  showArrows = stringToBoolean(showArrows);
  showPagination = stringToBoolean(showPagination);
</script>

<div class="image-gallery">
  <div
      class="image-pool"
      bind:this={imagePool}
      style="--slide-duration: {slideDuration}ms;
                --fade-duration: {fadeDuration}ms;"
  />
  {#if showArrows}
    <button type="button" class="prev" on:click|preventDefault={prev}>{@html chevron} Prev</button>
    <button type="button" class="next" on:click|preventDefault={next}>Next {@html chevron}</button>
  {/if}
  {#if showPagination}
    <GalleryPagination
      {images}
      {currentSlide}
      animationTime={slideDuration - fadeDuration}
      on:slideChange={handleSlideChange}
    />
  {/if}
</div>

<style>
    .image-pool {
        background-color: rgba(var(--skbs-background-color, 0, 0, 0, 1));
        bottom: 0;
        left: 0;
        overflow: hidden;
        position: absolute;
        right: 0;
        top: 0;
    }

    button {
        align-items: center;
        background-color: transparent;
        border: none;
        color: transparent;
        cursor: pointer;
        display: flex;
        height: 100%;
        position: absolute;
        top: 0;
        transition: opacity 1000ms ease;
        transition-delay: 700ms;
        width: 15vw;
        will-change: opacity;
        z-index: 1;
    }

    button :global(.icon) {
        fill: rgba(255, 255, 255, 0.35);
        height: auto;
        width: 20px;
    }

    button.prev {
        background-image: linear-gradient(90deg, rgba(0, 0, 0, 0.75) 0%, rgba(0, 0, 0, 0) 100%);
        justify-content: flex-start;
        left: 0;
        opacity: 0.5;
        padding-left: 2vw;
    }

    button.prev :global(.icon) {
        transform: rotate(180deg);
    }

    button.prev:hover {
        opacity: 1;
        transition: opacity 150ms ease;
        transition-delay: 0ms;
    }

    button.next {
        background-image: linear-gradient(-90deg, rgba(0, 0, 0, 0.75) 0%, rgba(0, 0, 0, 0) 100%);
        justify-content: flex-end;
        padding-right: 2vw;
        right: 0;
        opacity: 0.5;
    }

    button.next:hover {
        opacity: 1;
        transition: opacity 150ms ease;
        transition-delay: 0ms;
    }

    :global(.image-wrapper) {
        animation-duration: var(--slide-duration), var(--fade-duration);
        animation-timing-function: linear, ease;
        bottom: 0;
        left: 0;
        position: absolute;
        right: 0;
        top: 0;
        will-change: transform, opacity;
    }

    :global(.kenburns-image) {
        display: block;
        height: 100%;
        object-fit: cover;
        width: 100%;
    }

    @keyframes -global-ken-burns-center {
        to {
            transform: scale3d(1.3, 1.3, 1.3);
        }
    }

    @keyframes -global-ken-burns-top-right {
        to {
            transform: scale3d(1.3, 1.3, 1.3) translate3d(-10%, 7%, 0);
        }
    }

    @keyframes -global-ken-burns-top-left {
        to {
            transform: scale3d(1.3, 1.3, 1.3) translate3d(10%, 7%, 0);
        }
    }

    @keyframes -global-ken-burns-top-middle {
        to {
            transform: scale3d(1.3, 1.3, 1.3) translate3d(0, 10%, 0);
        }
    }

    @keyframes -global-ken-burns-bottom-right {
        to {
            transform: scale3d(1.3, 1.3, 1.3) translate3d(-10%, -7%, 0);
        }
    }

    @keyframes -global-ken-burns-bottom-left {
        to {
            transform: scale3d(1.3, 1.3, 1.3) translate3d(10%, -7%, 0);
        }
    }

    @keyframes -global-ken-burns-bottom-middle {
        to {
            transform: scale3d(1.3, 1.3, 1.3) translate3d(0, -10%, 0);
        }
    }

    @keyframes -global-ken-burns-middle-left {
        to {
            transform: scale3d(1.3, 1.3, 1.3) translate3d(10%, 0, 0);
        }
    }

    @keyframes -global-ken-burns-middle-right {
        to {
            transform: scale3d(1.3, 1.3, 1.3) translate3d(-10%, 0, 0);
        }
    }

    @keyframes -global-fade-in {
        from {
            opacity: 0;
        }

        to {
            opacity: 1;
        }
    }
</style>
