<style>
    :global(body){
        font-size: 16px;
        font-family: Nunito, Arial, sans-serif;
    }
    :global(.maincontainer ){
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		margin: 0.5em 0 3em 0;
	}
    :global(.maincontainer > *) {
        margin: 0.5em 0;
    }
	:global(.buttonbox .mainbutton) {
		margin-left: auto
	}
	:global(div.buttonbox) {
		display: flex;
		flex-direction: column;
		justify-content: right;
		background-color: black;
		color: white;
		padding: 20px;
	}
    :global(div.buttonbox.white){
        background-color: white;
        color: black;
    }
	:global(input,.mainbutton) {
		color: black;
		border: 1px solid black;
		border-radius: 6px;
		font-size: 1em;
		padding: 10px;
		background-color: var(--transparent);
	}
	
	:global(.mainbutton.white) {
		color: white;
		border: 1px solid white;
	}
	
	:global(input[type=text]) {
		border-radius: 0px;
		border-width: 0;
		border-bottom-width: 1px;
		border-bottom-style: dashed;
	}
</style>