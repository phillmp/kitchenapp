<script>
    export let icons;
    function onIconPress(){
        let lab = this.id.split("iconbutton_")[1];
        console.log("Pressed " + lab)
        const isequal = (element) => element.label === lab;
        let index = icons.findIndex(isequal)
        icons[index].selected = !icons[index].selected
    }

</script>

<div class=icongrid>
    {#each icons as icon}
        <button class="iconbutton{icon.selected ? ' selected' : ''}" 
            id="iconbutton_{icon.label}" on:click={onIconPress}>
        <div>
            {icon.icon}
        </div>
        </button>
    {/each}
</div>

<style>
    .iconbutton {
        color: black;
        border: 1px solid black;
        background-color: var(--transparent);
        border-radius: 4px;
        font-size: 2em;
        width: calc(2em);
        height: calc(2em);
        padding: 0;
        margin: 5px;
    }

    .iconbutton div {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    div.icongrid {
        display: flex;
        flex-flow: row wrap;
        max-width: calc(6* (2em + 7px));
        margin: 5px
    }

    .iconbutton.selected {
        background-color: #d7e7f3;
        border-color: #3a88c4;
    }
</style>