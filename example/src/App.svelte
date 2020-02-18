<script>

    // Normally, this would be import: `import SortableList from 'svelte-sortablejs';`
    import SortableList from "../../SortableList.svelte";

    let items = [1, 2, 3, 4].map(i => ({ identifierNumber: i, text: "item " + i }));

    function addItem() {
        if (items.length <= 0) items.push(1);
        else items.push(items[items.length - 1] + 1);
    }

    function removeItem() {
        items.pop();
    }

</script>

<h1>svelte-sortablejs example</h1>

<p>Try dragging them around!</p>

<SortableList bind:items idKey="identifierNumber" let:item>
    Hey, it's { item.text }!
</SortableList>


<button on:click={ addItem }>add item</button>
<button on:click={ removeItem }>remove item</button>
