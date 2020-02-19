<script>

	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

    import Sortable from 'sortablejs/modular/sortable.core.esm.js';

    // every item is an object with a unique ID for identification
    export let items = [];
    export let idKey = "id"; // id attribute changeable if needed
    export let ulClass = "";
    export let liClass = "";
    export let getItemById = undefined;
    export let sortableOptions = {};
    sortableOptions = Object.assign({}, sortableOptions); // prevent leak to outside
    if (sortableOptions.dataIdAttr) throw new Error("sortableOptions.dataIdAttr is currently not supported.");
    sortableOptions.store = sortableOptions.store || {
        set: sortable => undefined // placeholder
    };
    if (sortableOptions.group && !getItemById) {
        throw new Error("When using group, please provide a function called `getItemById` (as a prop) that gives an item in case it gets dropped from somewhere else. Otherwise, the SortableList cannot know what the item is exactly.");
    }
    getItemById = getItemById || ((id) => {
        return items.find(item => item[idKey] == id); // should only loosely check as IDs are auto-converted to strings
    });

    let _store_set = sortableOptions.store.set;
    sortableOptions.store.set = (sortable) => {
        items = sortable.toArray().map(getItemById);
        _store_set(sortable); // still call old set callback function
        dispatch("orderChanged", items);
    };
    
    let sortable;
    let listElement;
    $: if(listElement && !sortable) {
        sortable = Sortable.create(listElement, sortableOptions);
    }

    $: for (let item of items) {
        if (!item || item[idKey] == null) {
            throw new Error("Item " + item + " has no valid identifier " + idKey);
        }
    }

</script>


<ul bind:this={ listElement } class={ ulClass }>
    {#each items as item (item[idKey])}

	<li class={ liClass } data-id={ item[idKey] }>
        <slot { item }>{ item }</slot>
    </li>

    {/each}
</ul>
