<script>
  export let items;
  export let visibleindex;

  import { bounceOut } from "svelte/easing";
  import { crossfade } from "svelte/transition";
  import { flip } from "svelte/animate";
  import { onMount, beforeUpdate, tick } from "svelte";

  let contents;
  let visible;

  let leftlist = [];
  let centerlist = [];

  export function moveRight() {
    if (visibleindex < items.length - 1) {
      visibleindex++;
      leftlist = centerlist;
      centerlist = [items[visibleindex]];
    }
  }

  export function moveLeft() {
    if (visibleindex < items.length && visibleindex > 0) {
      visibleindex--;
      centerlist = [items[visibleindex]];
      if (visibleindex != 0) leftlist = [items[visibleindex - 1]];
      else leftlist = [];
    }
  }
  const [send, receive] = crossfade({
    fallback(node, params) {
      const style = getComputedStyle(node);
      const transform = style.transform === "none" ? "" : style.transform;

      return {
        duration: 600,
        easing: bounceOut,
        css: t => `
					transform: ${transform} scale(${t});					
				`
      };
    }
  });

  onMount(() => {
    items = items.map((data, i) => {
      return { index: i, data: { ...data, moveRight } };
    });
    centerlist = items && items.length ? [items[visibleindex]] : [];
    leftlist = visibleindex > 0 ? [items[visibleindex - 1]] : [];
    console.log({ items, centerlist, leftlist });
  });

  beforeUpdate(() => {
    console.log([items[visibleindex]], centerlist);
    if (visibleindex < 0) visibleindex = 0;
    if (visibleindex === items.length) visibleindex = items.length - 1;
    if (visibleindex === 0) {
      centerlist = [items[visibleindex]];
      leftlist = [];
    } else if (visibleindex <= items.length) {
      centerlist = [items[visibleindex]];
      leftlist = [items[visibleindex - 1]];
    }
  });
</script>

<style>
  carousel {
    display: flex;
    max-width: 60%;
  }

  @media screen and (max-width: 500px) {
    carousel {
      max-width: 100%;
    }
  }
  @media screen and (max-width: 800px) and (min-width: 500px) {
    carousel {
      max-width: 70%;
    }
  }
  .justifycenter {
    justify-content: flex-end;
  }

  .justifystart {
    justify-content: flex-end;
  }
</style>

<carousel
  bind:this={contents}
  class:justifycenter={leftlist.length == 0}
  class:justifystart={leftlist.length > 0}>
  {#each leftlist as row (row.index)}
    <carousel-item
      in:receive={{ key: row.index }}
      out:send={{ key: row.index }}
      animate:flip
      style={'opacity:0.4'}>
      <slot blah={row.data}>Missing template</slot>
    </carousel-item>
  {/each}
  {#each centerlist as row (row.index)}
    <carousel-item
      in:receive={{ key: row.index }}
      out:send={{ key: row.index }}
      animate:flip>
      <slot blah={row.data}>Missing template</slot>
    </carousel-item>
  {/each}
</carousel>
