<script>
	import Icongrid from './icongrid.svelte';

    export let itemparams;

    const urlparams = URLSearchParams(window.location.search)
    let itemid = urlparams.get('id')
    if (!itemid) {
        //error!
    }
    
</script>

<div id="viewer" class="maincontainer">
	<h1>{itemparams.name}</h1>
    <h3>{itemparams.expiry_date}</h3>
	<Icongrid icons={itemparams.icons} readonly=true/>
</div>

<div class="buttonbox">
	<button class="mainbutton white">Replace</button>
    <button class="mainbutton white">Edit</button>
	<button class="mainbutton white">Delete</button>
</div>

<style>
	.maincontainer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		margin: 0.5em 0 3em 0;
	}
	.buttonbox .mainbutton {
		margin-left: auto
	}

	div.buttonbox {
		display: flex;
		flex-direction: column;
		justify-content: right;
		background-color: black;
		color: white;
		padding: 20px;
	}
	input,.mainbutton {
		color: black;
		border: 1px solid black;
		border-radius: 6px;
		font-size: 1em;
		padding: 10px;
		background-color: var(--transparent);
	}
	
	.mainbutton.white {
		color: white;
		border: 1px solid white;
	}
	
	input[type=text] {
		border-radius: 0px;
		border-width: 0;
		border-bottom-width: 1px;
		border-bottom-style: dashed;
	}
    </style>