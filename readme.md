<p align="center">
  <img src="static/favicon.svg" alt="Svelte Bricks" height=150>
</p>

# Svelte Bricks

[![NPM version](https://img.shields.io/npm/v/svelte-bricks?color=blue&logo=NPM)](https://npmjs.com/package/svelte-bricks)
[![Netlify Status](https://api.netlify.com/api/v1/badges/c3213069-e3cc-45ef-a446-b2358b9a35fb/deploy-status)](https://app.netlify.com/sites/svelte-bricks/deploys)

This is a naive masonry implementation in Svelte without column balancing. [Live demo](https://svelte-bricks.netlify.app).

## Installation

```sh
yarn add -D svelte-bricks
```

## Usage

The kitchen sink for this component looks something like this:

```svx
<script>
  import Masonry from 'svelte-bricks'

  let nItems = 30
  $: items = Array(nItems).fill(null).map((_, idx) => idx + 1)

  let [minColWidth, maxColWidth, gap] = [200, 800, 20]
  let width, height
</script>

Masonry size: <span>{width}px</span> &times; <span>{height}px</span> (w &times; h)

<Masonry {items} {minColWidth} {maxColWidth} {gap} let:item bind:width bind:height>
  <Some {item} />
</Masonry>
```

**Note**: On non-primitive types, i.e. if `items` is an array of objects, this component requires that each object have a key named `'id'` that contains a unique primitive value. This value is used to identify each item in the keyed `{#each}` block that renders the masonry layout. Without this, Svelte could not avoid duplicates when new items are added nor maintain order when existing ones are rearranged. [See the docs for details](https://svelte.dev/tutorial/keyed-each-blocks).

`Masonry.svelte` has 8 props (plus a slot), 7 of which are optional. It expects an array of `items` as well as a slot component used to render each of the items. The array can contain whatever data (objects, strings, numbers) as long as the slot component knows how to handle it. The optional props are:

- `minColWidth: number = 330` (in `px`)
- `maxColWidth: number = 500` (in `px`)
- `gap: number = 20` (in `px`)
- `masonryWidth: number = 0`: Bound to the masonry `div`s width (in `px`).
- `masonryHeight: number = 0`: Bound to the masonry `div`s height (in `px`).
- `animate: boolean = true`: Whether to [FLIP-animate](https://svelte.dev/tutorial/animate) masonry items when viewport resizing or other events cause `items` to rearrange.
