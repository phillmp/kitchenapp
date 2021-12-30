<script>
	import Icongrid from './icongrid.svelte';
	let toplevelicons = [
		{
			'label': 'meat',
			'icon':'🍖',
			'selected': false,
		},
		{
			'label': 'egg',
			'icon':'🍳',
			'selected': false,
		},
		{
			'label': 'milk',
			'icon':'🥛',
			'selected': false,
		},
		{
			'label': 'wheat',
			'icon':'🌾',
			'selected': false,
		},
		{
			'label': 'onion',
			'icon':'🧅',
			'selected': false,
		},
		{
			'label': 'garlic',
			'icon':'🧄',
			'selected': false,
		},
		{
			'label': 'spicy',
			'icon':'🌶',
			'selected': false,
		},
		{
			'label': 'peanuts',
			'icon':'🥜',
			'selected': false,
		},
		{
			'label': 'lock',
			'icon':'🔒',
			'selected': false,
		},
	]
</script>
<div id="maincontainer">
	<input type="text" id="title" name="title"
	placeholder="No name">
	<input type="date" id="expiry_date" name="expiry_date">
	<Icongrid icons={toplevelicons}/>
</div>

<div class="buttonbox">
	<button class="mainbutton white">hello nerd</button>
	<button class="mainbutton white">Daddy love you very much baby girl</button>
</div>

<!-- 
window.onload = function () {
    firebase.auth().onAuthStateChanged(function(user) {
        if (user) {
            //user is signed in
        }
        else {
            //user is signed out
        }
    });
    } -->


<style>
	#maincontainer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		margin: 0.5em 0 3em 0;
	}
	/* #maincontainer > div {
		font-size: 15pt;
		margin: 10px 0;
	} */
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
	
	input[type=text]{
		border-radius: 0px;
		border-width: 0;
		border-bottom-width: 1px;
		border-bottom-style: dashed;
	}

	#title {
		max-width:10em;
		font-size:1.4em;
		margin: 20px 0;
		text-align: center;
	}

</style>
<!-- <main>

</main> -->