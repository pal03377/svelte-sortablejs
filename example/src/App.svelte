<script>

    // Normally, this would be import: `import SortableList from '@palsch/svelte-sortablejs';`
    import SortableList from "../../SortableList.svelte";

    let items = [1, 2, 3, 4].map(i => ({ identifierNumber: i, text: "item " + i }));
    let items2 = [-1, -2, -3, -4].map(i => ({ identifierNumber: i, text: "item " + i }));
    let currentNumber = 5;

    const sortableOptions = {
        group: "items", 
        animation: 100, 
        easing: "cubic-bezier(1, 0, 0, 1)"
    };

    function addItem() {
        items.push({
            identifierNumber: currentNumber, 
            text: "item " + currentNumber
        });
        currentNumber ++;
        items = items; // refresh UI
    }

    function removeItem() {
        items.pop();
        items = items; // refresh UI
    }

    function itemOrderChanged(event) {
        console.log("item order changed!", event.detail);
    }

    function getItemById(id) {
        return items.concat(items2).find(item => item.identifierNumber == id);
    }

</script>

<h1>svelte-sortablejs example</h1>

<h2>Normal</h2>

<p>Try dragging them around!</p>

<SortableList { sortableOptions } on:orderChanged={ itemOrderChanged } 
        bind:items idKey="identifierNumber" let:item { getItemById }
        liClass="largerFont">
    Hey, it's { item.text }!
</SortableList>


<button on:click={ addItem }>add item</button>
<button on:click={ removeItem }>remove item</button>

<br><br>
<hr>
<br><br>

<h2>Multiple groups</h2>
<p>Dragging between groups is supported as well. Try dragging these to the list above and vice-versa!
When dragging items from above, they should still be black. When dragging items from below into the above field, they should get black.</p>
<p>As the text before the item name is specified within a slot, it should always be consistent within each block here.</p>

<SortableList { sortableOptions } on:orderChanged={ itemOrderChanged } 
        bind:items={ items2 } idKey="identifierNumber" let:item { getItemById } 
        liClass={ /* liClass can be a function too - then it gets the id of the item */ (item) => item.identifierNumber >= 0 ? "largerFont" : "largerFont crazy" }>
    Welcome in the bottom section, { item.text }!
</SortableList>

<style>

    /* you need to use :global so that Svelte does not optimize away the in its view "unused" classes
       see https://github.com/sveltejs/svelte/issues/2870 */
    :global(.largerFont) {
        font-size: 1.5rem;
    }

    :global(.crazy) {
        color: red;
    }

</style>
