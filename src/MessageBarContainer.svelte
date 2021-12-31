
<script>
    import Messagebar from "./Messagebar.svelte"
    export let mbevents;

    function deleteMessage(event){
        // delete first element in arr
        mbevents = mbevents.splice(1)
    }	
</script>


<div class="messagebarcontainer">
{#each mbevents as mbevent}
	<Messagebar {mbevent} on:delete={deleteMessage}/>
{/each}
</div>

<style>
    .messagebarcontainer {
        position: absolute;
        top:0;
        width:100%;
        display:flex;
        flex-direction: column;
        z-index:1000;
    }
</style>