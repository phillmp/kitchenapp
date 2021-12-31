<script>
    import { createEventDispatcher } from 'svelte';
    import { getDatabase, ref, child, get, set, onValue} from "firebase/database";
    import Icongrid from "./Icongrid.svelte"
    import { eventStates } from "./Messagebar.svelte"
    
    export let app;
    export let auth;
    export let itemid;

    let loadedOnceOrMore=false;
    let newentry=false;
    let entryNotExist=false;

	const dispatch = createEventDispatcher();

    //TODO could probably prototype viewerEvent
    const dispatchEvents = {
        'save': {
            'failed':{
                'viewerEvent':{
                    'message': "Save failed!",
                    'state': eventStates.fail
                }
            },
            'success':{
                'viewerEvent':{
                    'message': "Saved!",
                    'state': eventStates.pass
                }
            }
        },
    }

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
    
    function getisexpired(xd){
        var today = new Date()
        var isexpired = Date.parse(xd+" 23:59:59") < today.valueOf() 
        return xd && isexpired;
    }
    function getissafe(xd){
        var today = new Date()
        var isexpired = Date.parse(xd+" 23:59:59") > today.valueOf() 
        return xd && isexpired;
    }
    $: isexpired = getisexpired(itemparams.expiry_date)
    $: issafe = getissafe(itemparams.expiry_date)
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
                console.log(isexpired)
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
        }).then( ()=>{
            var msg = {'message': "Saved!",
                    'state': eventStates.pass}
            console.log("sending event" + JSON.stringify(msg))
            dispatch('viewerEvent', msg)
        }).catch( ()=>{
            var msg = {'message': "Save failed!",
                    'state': eventStates.fail}
            console.log("sending event" + JSON.stringify(msg))
            dispatch(dispatchEvents.save.failed)
        })
    }
    
</script>

{#if loadedOnceOrMore === itemid && !entryNotExist}
<div class="maincontainer">
	<input type="text" id="name" class="dashed"
	placeholder="{newentry ? "New item" : "No name"}" bind:value={itemparams.name}>
	<div>
        <label for="expiry_date">Expiry date</label>
        <input type="date" id="expiry_date" name="expiry_date"
        placeholder="yyyy-mm-dd"
        bind:value={itemparams.expiry_date}
        class:expired={isexpired}
        class:safe={issafe}
        >
    </div>
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
