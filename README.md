# Svelte Ken Burns Slideshow.
This is a simple and preformat Svelte component that creates an Image SlideShow with the well known Ken Burns effect. 

## svelte-ken-burns-slideshow
The Ken Burns effect is a type of panning and zooming effect used in film and video production from still imagery. The name derives from extensive use of the technique by American documentarian Ken Burns. This technique had also been used to produce animatics, simple animated mockups used to previsualize motion pictures, but Burns's name has become associated with the effect in much the same way as Alfred Hitchcock is associated with the dolly zoom.

Check Wikipedia: https://en.wikipedia.org/wiki/Ken_Burns_effect

### Installation
https://www.npmjs.com/package/svelte-ken-burns-slideshow
```sh
npm i svelte-ken-burns-slideshow --save-dev
```

### Usage 
#### Fullscreen view.
```svelte
<script>
    import KenBurns from 'svelte-ken-burns-slideshow';
    
    const gallery = [
        './gallery/01.jpg',
        './gallery/02.jpg',
        './gallery/03.jpg',
        './gallery/04.jpg',
        './gallery/05.jpg',
    ];
</script>

<KenBurns images={gallery} />

```

#### Boxed view.
By default the ImageSlideShow runs in fullscreen. If you need a specific size of your SlideShow, just it in a parent container with the dimensions you like and a position relative.

```svelte

<div>
    <KenBurns images={gallery} />
</div>

<style>
    div {
        position: relative;
        height: 300px;
        width: 400px;
    }
</style>

```
### Parameters

```svelte
<KenBurns images={gallery} slideDuration="3000" fadeDuration="500" showArrows="false" />
```

| Parameter | Default | Description | Unit |
| --- | --- | --- | --- |
| images | [] | Array with image paths as string. | ['...'] |
| slideDuration | 25000 | Time in milliseconds for the Ken Burns effect itself (zoom in / out) + motion. | ms |
| fadeDuration | 5000 | Time in milliseconds for fade-in effect when the image appear. | ms |
| showArrows | true | Show or hide arrows on left and right to navigate. | boolean |
| showNavigation | --- | TBD coming soon.

## Demo
Coming soon.

### Demo on local

Coming soon.
