<script>
import { reload } from "firebase/auth";

    import { getDatabase, ref, child, get, set, onValue} from "firebase/database";
    import Icongrid from "./Icongrid.svelte"
    
    export let app;
    export let auth;
    export let itemid;
    let loadedOnceOrMore=false;
    let newentry=false;
    let entryNotExist=false;
    function formatDate(date){
        var dd = String(date.getDate()).padStart(2, '0');
        var mm = String(date.getMonth() + 1).padStart(2, '0'); //January is 0!
        var yyyy = date.getFullYear();
        return yyyy + "-" + mm + "-" + dd;
    }
    function InitExpiry(){
        var date = new Date();
        date.setDate(date.getDate()+7)
        var datestr = formatDate(date)
        return ""
    }
    let itemparams = {
        'home_id': "",
        'expiry_date': InitExpiry(),
        'name': "",
        'tags': [],
    }
    let today = new Date()
    //todo merge state variables into an object
    $: isexpired = itemparams.expiry_date && Date.parse(itemparams.expiry_date) < today;
    $: issafe = itemparams.expiry_date && Date.parse(itemparams.expiry_date) > today
    app;
    auth;
    const db = getDatabase()
    get(child(ref(db), 'Items/' + itemid)).then( (snapshot)  => {
        loadedOnceOrMore = itemid;        
        console.log("yep i ran")
        if (snapshot.exists() && snapshot.val().home_id) {
            console.log(snapshot.val().name)
            itemparams.home_id = snapshot.val().home_id
            itemparams.expiry_date = snapshot.val().expiry_date
            
            
            if (snapshot.val().expiry_date){
                itemparams.expiry_date = snapshot.val().expiry_date;
                itemparams.name = snapshot.val().name
            }
            else {
                newentry = true
                itemparams.expiry_date = "";
                itemparams.name = ""
            }
            if ('tags' in snapshot.val()){
                itemparams.tags = snapshot.val().tags
            }
            else {
                itemparams.tags = []
            }
    }
    else{
        entryNotExist=true;
        console.log(entryNotExist)
    }
    });
    function Clear(app, auth, db){
        app;
        auth;
        db;
        itemparams = {
            'home_id': itemparams.home_id,
            'expiry_date': InitExpiry(),
            'name': "",
            'tags': [],
        }
        Save(app, auth, db)
        location.reload()
    }
    function Save(app, auth, db){
        app;
        auth;
        console.log(JSON.stringify(itemparams))
        set(ref(db, 'Items/'+ itemid,), {
            expiry_date: itemparams.expiry_date,
            name: itemparams.name,
            tags: itemparams.tags,
            home_id: itemparams.home_id
        })
    }
</script>

{#if loadedOnceOrMore === itemid && !entryNotExist}
<div class="maincontainer">
	<input type="text" id="title" name="title"
	placeholder="{newentry ? "New item" : "No name"}" bind:value={itemparams.name}>
	<input type="date" id="expiry_date" 
    name="expiry_date" bind:value={itemparams.expiry_date}
    class:expired={isexpired}
    class:safe={issafe}
    >
	<Icongrid tags={itemparams.tags}/>
</div>

<div class="buttonbox">
	<button class="mainbutton white" on:click={Save(app, auth, db)}>Save</button>
    <button class="mainbutton white" on:click={Clear(app, auth, db)}>Clear</button>
</div>
{:else if entryNotExist}
<div class="maincontainer">
    That item doesn't exist. Sorry!
</div>
{/if}

<style>
    input[type=date] {
        border: 1px black solid;
    }
    input[type=date].expired {
        border-width:3px;
        border-color: red;
    }
    input[type=date].safe {
        border-width:3px;
        border-color: green;
    }
</style>
