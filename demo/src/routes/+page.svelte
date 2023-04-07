<script>
  import KenBurns from '../../../KenBurns.svelte';

  const gallery = [
    'images/example-gallery/03.jpg',
    'images/example-gallery/05.jpg',
    'images/example-gallery/07.jpg',
  ];

  const demoStyles1 = `
    .gallery-holder {
       position: relative;
       max-width: 75vw;
       aspect-ratio: 4 / 3;
    }
  `;

  const demoMarkup1 = `
    <div class="gallery-holder">
      <KenBurns images={gallery} />
    </div>
  `;

  const demoStyles2 = `
    .gallery-holder {
       position: relative;
       max-width: 75vw;
       aspect-ratio: 16 / 9;
    }
  `;

  const demoMarkup2 = `
    <div class="gallery-holder">
      <KenBurns
        slideDuration="15000"
        fadeDuration="3000"
        images={gallery}
        --skbs-progress-bar-color="255, 255, 0, 0.5"
        --skbs-pagination-offset-bottom="3px"
      />
    </div>
  `;

  const demoStyles3 = `
    .gallery-holder {
       position: relative;
       height: 300px;
       width: 300px;
    }
  `;

  const demoMarkup3 = `
    <div class="gallery-holder">
      <KenBurns
        showArrows="false"
        showPagination="false"
        slideDuration="10000"
        images={gallery}
      />
    </div>
  `;

  const shuffleArray = (array) => {
    let currentIndex = array.length;
    let randomIndex;
    let shuffledArray = [...array];

    while (currentIndex !== 0) {
      randomIndex = Math.floor(Math.random() * currentIndex);
      currentIndex--;

      [shuffledArray[currentIndex], shuffledArray[randomIndex]] = [
        shuffledArray[randomIndex],
        shuffledArray[currentIndex],
      ];
    }

    return shuffledArray;
  };
</script>

<h1>Svelte Ken Burns Slideshow Demo</h1>

<h2>Demo 1</h2>
<div class="gallery-holder demo1">
  <KenBurns images={shuffleArray(gallery)} />
</div>

<pre>
   <code>
      {demoStyles1}
   </code>
</pre>

<pre>
   <code>
      {demoMarkup1}
   </code>
</pre>

<h2>Demo 2</h2>
<div class="gallery-holder demo2">
  <KenBurns
    slideDuration="15000"
    fadeDuration="3000"
    images={shuffleArray(gallery)}
    --skbs-progress-bar-color="255, 255, 0, 0.5"
    --skbs-pagination-offset-bottom="3px"
  />
</div>

<pre>
   <code>
      {demoStyles2}
   </code>
</pre>

<pre>
   <code>
      {demoMarkup2}
   </code>
</pre>

<h2>Demo 3</h2>
<div class="gallery-holder demo3">
  <KenBurns
    showArrows="false"
    showPagination="false"
    slideDuration="10000"
    images={shuffleArray(gallery)}
  />
</div>

<pre>
   <code>
      {demoStyles3}
   </code>
</pre>

<pre>
   <code>
      {demoMarkup3}
   </code>
</pre>

<style>
  h2 {
    margin-top: 3em;
  }

  .gallery-holder {
    margin-bottom: 1rem;
    margin-right: auto;
    margin-left: auto;
    max-width: 75vw;
    position: relative;
  }

  .gallery-holder.demo1 {
    aspect-ratio: 4 / 3;
  }

  .gallery-holder.demo2 {
    aspect-ratio: 16 / 9;
  }

  .gallery-holder.demo3 {
    height: 300px;
    width: 300px;
  }

  pre {
    font-family: monospace;
    background-color: lightgray;
    font-size: 1.2em;
    border: 1px solid #999;
    color: black;
    line-height: 1.5;
    width: 500px;
    border-radius: 2px;
    box-shadow: 1px 1px 5px rgb(0 0 0 / 30%);
    text-align: left;
    margin-left: auto;
    margin-right: auto;
  }
</style>
