WIP

- This is a work in progress , inspired by need to have an interactive carousel with more than one visible section within the window.
- Also to have buttons which provide options within the carousel to move back and forth or to index

TODO

- [ ] Update documentation with clear examples
- [ ] write tests

## Setting up

```
npm i @siddharatha/svelte-interactive-carousel
```

```html
<script>
  import { data } from "./data.js";
  import Carousel from "svelte-interactive-carousel";
  import CarouselItem from "./CarouselItem.svelte";
  import Paginator from "./Paginator.svelte";

  export let visibleindex = 0;
  let carousel;
</script>

<Carousel items="{data}" let:blah bind:visibleindex bind:this="{carousel}">
  <CarouselItem {...blah} />
</Carousel>
<button on:click="{carousel.moveLeft}">&lt-</button>
<button on:click="{carousel.moveRight}">-&gt</button>
```

## Consuming components

Your package.json has a `"svelte"` field pointing to `src/index.html`, which allows Svelte apps to import the source code directly, if they are using a bundler plugin like [rollup-plugin-svelte](https://github.com/rollup/rollup-plugin-svelte) or [svelte-loader](https://github.com/sveltejs/svelte-loader) (where [`resolve.mainFields`](https://webpack.js.org/configuration/resolve/#resolve-mainfields) in your webpack config includes `"svelte"`). **This is recommended.**

For everyone else, `npm run build` will bundle your component's source code into a plain JavaScript module (`index.mjs`) and a UMD script (`index.js`). This will happen automatically when you publish your component to npm, courtesy of the `prepublishOnly` hook in package.json.
