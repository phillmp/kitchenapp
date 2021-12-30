<script>
import { reload } from "firebase/auth";

    import { getDatabase, ref, child, get, set, onValue} from "firebase/database";
    import Icongrid from "./Icongrid.svelte"
    
    export let app;
    export let auth;
    export let itemid;
    let loadedOnceOrMore=false;
    function InitExpiry(){
        var date = new Date();
        date.setDate(date.getDate()+7)
        var dd = String(date.getDate()).padStart(2, '0');
        var mm = String(date.getMonth() + 1).padStart(2, '0'); //January is 0!
        var yyyy = date.getFullYear();
        return ""
        // return yyyy + "-" + mm + "-" + dd;
    }
    let itemparams = {
        'home_id': "",
        'expiry_date': InitExpiry(),
        'name': "",
        'tags': [],
    }
    app;
    auth;
    const db = getDatabase()
    get(child(ref(db), 'Items/' + itemid)).then( (snapshot)  => {
        loadedOnceOrMore = itemid;        
        console.log("yep i ran")
        if (snapshot.exists()) {
            console.log(snapshot.val().name)
            itemparams.home_id = snapshot.val().home_id
            itemparams.expiry_date = snapshot.val().expiry_date
            itemparams.name = snapshot.val().name
            
            if (snapshot.val().expiry_date){
                itemparams.expiry_date = snapshot.val().expiry_date;
            }
            else {
                itemparams.expiry_date = InitExpiry();
            }
            if ('tags' in snapshot.val()){
                itemparams.tags = snapshot.val().tags
            }
            else {
                itemparams.tags = []
            }
    }});
    function Clear(app, auth, db){
        app;
        auth;
        db;
        itemparams = {
            'home_id': "",
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

{#if loadedOnceOrMore === itemid}
<div class="maincontainer">
	<input type="text" id="title" name="title"
	placeholder="No name" bind:value={itemparams.name}>
	<input type="date" id="expiry_date" name="expiry_date" bind:value={itemparams.expiry_date}>
	<Icongrid tags={itemparams.tags}/>
</div>

<div class="buttonbox">
	<button class="mainbutton white" on:click={Save(app, auth, db)}>Save</button>
    <button class="mainbutton white" on:click={Clear(app, auth, db)}>Clear</button>
</div>
{/if}