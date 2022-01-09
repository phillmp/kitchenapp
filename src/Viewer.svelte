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
    let showExpiry=false

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

    function stripTimeFromDate(d){
        var r = new Date(d.getFullYear(), d.getMonth(), d.getDate())
        return r
    }
    function getToday(){
        var now = new Date
        return stripTimeFromDate(now)
    }

    function formatDate(date){
        var dd = String(date.getDate()).padStart(2, '0');
        var mm = String(date.getMonth() + 1).padStart(2, '0'); //January is 0!
        var yyyy = date.getFullYear();
        return yyyy + "-" + mm + "-" + dd;
    }
    function dateDaysFromToday(days){
        var date = getToday();
        date.setDate(date.getDate()+days)
        var datestr = formatDate(date)
        return datestr
    }

    function parseDate(str) {
        var mdy = str.split('-');
        return new Date(mdy[0], mdy[1]-1, mdy[2]);
    }

    function datediff(first, second) {
        // Take the difference between the dates and divide by milliseconds per day.
        // Round to nearest whole number to deal with DST.
        console.log(first)
        console.log(second)
        return Math.abs(Math.floor((stripTimeFromDate(second)-stripTimeFromDate(first))/(1000*60*60*24)));
    }
    let itemparams = {
        'home_id': "",
        'expiry_date': "",
        'checkin_date': "",
        'name': "",
        'tags': [],
    }
    
    function getisexpired(xd){
        var today = getToday()
        var isexpired = Date.parse(xd+" 23:59:59") < today.valueOf() 
        return xd && isexpired;
    }
    function getissafe(xd){
        var today = getToday()
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
        if (snapshot.exists() && snapshot.val().home_id) {
            console.log(snapshot.val().name)
            itemparams.home_id = snapshot.val().home_id
            itemparams.expiry_date = snapshot.val().expiry_date
            
            if (snapshot.val().checkin_date){
                itemparams.checkin_date = snapshot.val().checkin_date;
                itemparams.name = snapshot.val().name
                newentry = false;
            }
            else {
                newentry = true
                itemparams.checkin_date = dateDaysFromToday(0);
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
    }
    });
    function Clear(app, auth, db){
        app;
        auth;
        db;
        itemparams = {
            'home_id': itemparams.home_id,
            'expiry_date': "",
            'checkin_date': "",
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
            checkin_date: itemparams.checkin_date,
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
        newentry=false;
    }
    
</script>

{#if loadedOnceOrMore === itemid && !entryNotExist}
<div class="maincontainer">
	<input type="text" id="name" class="dashed"
	placeholder="{newentry ? "New item" : "No name"}" bind:value={itemparams.name}>
	<div>
        {#if !newentry}
        <!-- <label for="checkin" >Added</label> -->
        <div id="checkin">
        Added on {itemparams.checkin_date}<br>
        ({datediff(getToday(), parseDate(itemparams.checkin_date))} days ago)
        </div>
        {/if}
        {#if showExpiry}
        <label for="expiry_date">Expires</label>
        <input type="date" id="expiry_date" name="expiry_date"
        placeholder="yyyy-mm-dd"
        bind:value={itemparams.expiry_date}
        class:expired={isexpired}
        class:safe={issafe}
        >
        {/if}
    </div>
	<Icongrid tags={itemparams.tags}/>
</div>

<div class="buttonbox">
	<button class="mainbutton white" on:click={Save(app, auth, db)}>Save</button>
    {#if !newentry}
    <button class="mainbutton white" on:click={Clear(app, auth, db)}>Clear</button>
    {/if}
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
    #checkin {
        text-align: center;
    }
</style>
