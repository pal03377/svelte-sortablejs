<script>

    // Normally, this would be import: `import SortableList from '@palsch/svelte-sortablejs';`
    import SortableList from "../../SortableList.svelte";

    let items = [1, 2, 3, 4].map(i => ({ identifierNumber: i, text: "item " + i }));
    let currentNumber = 5;

    const sortableOptions = {
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

    function itemOrderChanged(newItems) {
        console.log("item order changed!", newItems);
    }

</script>

<h1>svelte-sortablejs example</h1>

<p>Try dragging them around!</p>

<SortableList { sortableOptions } on:orderChanged={ itemOrderChanged } bind:items idKey="identifierNumber" let:item>
    Hey, it's { item.text }!
</SortableList>


<button on:click={ addItem }>add item</button>
<button on:click={ removeItem }>remove item</button>
