<script>
	import { initializeApp } from "firebase/app";
    import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, signOut } from "firebase/auth";

	import Login from "./Login.svelte"
	import Viewer from "./Viewer.svelte"
	import Theme from "./Theme.svelte"
	let params = new URLSearchParams(document.location.search);
	let itemid = params.get("id"); // is the string "Jonathan"
	let cur_uid = ''
	let loggedin = false;
	let fbloaded = false;

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
		fbloaded = true;
		if (user) {
			cur_uid = user.uid;
		} else {
			cur_uid = '';
		}
	});

</script>
<!-- <ul>
	<li>Yep it's printing stuff</li>
	<li>Your user id is {cur_uid}</li>
	<li>Current item id is {itemid}</li>
</ul> -->
<Theme/>
<div id="toplevel">
{#if !cur_uid && fbloaded}
<Login/>
{:else if fbloaded}
<Viewer app={fbapp} auth={auth} itemid={itemid}/>
<div class="buttonbox white">
	<button class="mainbutton" on:click={logout}>Log out</button>
</div>

{:else}
<!-- show loading spinner -->
{/if}
</div>

<style>
	#toplevel {
		border: 1px solid black;
		padding: 2em 0 0 0;
	}
</style>