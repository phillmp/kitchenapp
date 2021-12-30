<script>
    export let tags;
    function onIconPress(tag){
        const isequal = (element) => element.tag === tag;
        let index = icons.findIndex(isequal)
        icons[index].selected = !icons[index].selected

        if (tags.includes(tag)) {
            //enabled item was tapped - disable it
            let i = tags.indexOf(tag)
            tags.splice(i, 1)
            tags = tags //trigger refresh
        } else {
            tags.push(tag)
            tags = tags //trigger refresh
        }
    }
    let icons= [
		{
			'tag': 'meat',
			'icon':'🍖',
			'selected': false,
		},
		{
			'tag': 'egg',
			'icon':'🍳',
			'selected': false,
		},
		{
			'tag': 'milk',
			'icon':'🥛',
			'selected': false,
		},
		{
			'tag': 'wheat',
			'icon':'🌾',
			'selected': false,
		},
		{
			'tag': 'onion',
			'icon':'🧅',
			'selected': false,
		},
		{
			'tag': 'garlic',
			'icon':'🧄',
			'selected': false,
		},
		{
			'tag': 'spicy',
			'icon':'🌶',
			'selected': false,
		},
		{
			'tag': 'peanuts',
			'icon':'🥜',
			'selected': false,
		},
		{
			'tag': 'lock',
			'icon':'🔒',
			'selected': false,
		},
	];
    //initialize selections
    for (let i = 0; i < icons.length; i++){
            icons[i].selected=tags.includes(icons[i].tag);
        }
</script>

<div class=icongrid>
    {#each icons as icon}
        <button class="iconbutton" class:selected="{icon.selected}" 
            id="iconbutton_{icon.tag}" on:click={onIconPress(icon.tag)}>
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