<script>
	import { initializeApp } from "firebase/app";
    import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, signOut } from "firebase/auth";

	import Login from "./Login.svelte"
	import Viewer from "./Viewer.svelte"
	import LoadingSpinner from "./LoadingSpinner.svelte"
	import {MessageBarEvent, eventStates} from "./Messagebar.svelte"
	import MessageBarContainer from "./MessageBarContainer.svelte"
	
	let params = new URLSearchParams(document.location.search);
	let itemid = params.get("id");
	let cur_user = {
		"uid":'',
		'email':''
	}
	let fbloaded = false;
	let event_user_notifications = [];

	const firebaseConfig = {
        apiKey: "AIzaSyBUkJT0AEzVrGAms1WbDAbn_dTNehXbPNw",
        authDomain: "kitchenapp-334918.firebaseapp.com",
        projectId: "kitchenapp-334918",
        storageBucket: "kitchenapp-334918.appspot.com",
        messagingSenderId: "20897173184",
        appId: "1:20897173184:web:ca123ab0bfd13ffc9ca725",
        measurementId: "G-QHMM1CJLLL"
        };
	
	const fbapp = initializeApp(firebaseConfig);
	const auth = getAuth();

	async function logout(){
		const auth = getAuth();
        signOut(auth).then(() => {
            console.log("signed out");
        }).catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
            console.log(errorCode);
            console.log(errorMessage);
        });
	}

	onAuthStateChanged(auth, (user) => {
		// This will fire on initial load then also
		// whenever there is a change to the login state
		fbloaded = true;
		if (user) {
			cur_user.uid = user.uid;
			cur_user.email = user.email;
		} else {
			cur_user.uid = '';
			cur_user.email = '';
		}
	});

	function handleViewerEvent(event){
		console.log('received a message:'+ JSON.stringify(event.detail))
		var formatted_event = new MessageBarEvent(event.detail.message, event.detail.state);
		event_user_notifications = [...event_user_notifications, formatted_event];
	}
</script>

<div id="appcontent">
<MessageBarContainer mbevents={event_user_notifications}/>
{#if !cur_user.uid && fbloaded}
<Login/>
{:else if fbloaded}
	{#if itemid}
		<Viewer app={fbapp} auth={auth} itemid={itemid} on:viewerEvent={handleViewerEvent}/>
		<div class="buttonbox white">
			<button class="mainbutton" on:click={logout}>Log out</button>
		</div>
	{:else}
		Nothing here yet!
	{/if}
{:else}
<LoadingSpinner/>
{/if}
</div>

<style>
	#appcontent {
		border: 1px solid black;
		padding: 2em 0 0 0;
	}
</style>