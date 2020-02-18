<script>

    import Sortable from 'sortablejs/modular/sortable.core.esm.js';

    // every item is an object with a unique ID for identification
    export let items = [];
    export let idKey = "id"; // id attribute changeable if needed
    export let ulClass = "";
    export let liClass = "";
    export let sortableOptions = {};
    sortableOptions.store = sortableOptions.store || {
        set: sortable => undefined // placeholder
    };

    function itemsToObject() {
        // returns an object of items such that Object.values(items) == items
        // and the keys are the IDs
        let obj = {};
        for (let item of items) {
            obj[item[idKey]] = item;
        }
        return obj;
    }

    let _store_set = sortableOptions.store.set;
    sortableOptions.store.set = (sortable) => {
        let itemObj = itemsToObject();
        items = sortable.toArray().map(id => itemObj[id]);
        _store_set(sortable); // still call old set callback function
    };
    
    let sortable;
    let listElement;
    $: if(listElement) sortable = Sortable.create(listElement, sortableOptions);

    $: console.log(items);

</script>


<ul bind:this={ listElement } class={ ulClass }>
    {#each items as item (item[idKey])}

	<li class={ liClass } data-id={ item[idKey] }>
        <slot { item }>{ item }</slot>
    </li>

    {/each}
</ul>
