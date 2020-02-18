# svelte-sortablejs
Basic Svelte version of https://sortablejs.github.io/Sortable/

This is a basic Svelte component that can display a sortable list with the help of [Sortable.js](https://sortablejs.github.io/Sortable/).

## Demo
[The demo in the example folder is online to try out directly.](https://pal03377.github.io/svelte-sortablejs/)

## Usage
`npm install --save @palsch/svelte-sortablejs`

then import: `import SortableList from 'svelte-sortablejs';`

The `SortableList` component expects to get `items` as a list of objects, each having an attribute `id` (can be renamed with the component property `idKey`). I recommend to just bind to `items`. 
You can also pass options to sortable itself with `sortableOptions`. A `SortableList` can also be given inner HTML to display the item. Use `let:item` as component attribute for that.

The `SortableList` will give you a HTML structure based on `<ul>` and `<li>`s. You can style these by passing an `ulClass` or `liClass`.

Whenever the order of items is changed, the `orderChanged` event is fired with the new items.

Example usage (see also [the example](example/src/App.svelte)):

```
<SortableList { sortableOptions } on:orderChanged={ itemOrderChanged } bind:items idKey="identifierNumber" let:item>
    Hey, it's { item.text }!
</SortableList>
```
