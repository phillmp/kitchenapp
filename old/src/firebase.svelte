<script>
    export let authenticated_user; //uid if logged in; null if not
    export let prev_URL; //string containing URL before sign in page
    var firebase = require('firebase');
    var firebaseui = require('firebaseui');
    import { FirebaseError, initializeApp } from 'firebase/app';
    const firebaseConfig = {
        apiKey: "AIzaSyBUkJT0AEzVrGAms1WbDAbn_dTNehXbPNw",
        authDomain: "kitchenapp-334918.firebaseapp.com",
        projectId: "kitchenapp-334918",
        storageBucket: "kitchenapp-334918.appspot.com",
        messagingSenderId: "20897173184",
        appId: "1:20897173184:web:ca123ab0bfd13ffc9ca725",
        measurementId: "G-QHMM1CJLLL"
    };
    var ui = new firebaseui.auth.AuthUI(firebase.auth());
    var uiConfig = {
            callbacks: {
            signInSuccessWithAuthResult: function(authResult, redirectUrl) {
                // User successfully signed in.
                // Return type determines whether we continue the redirect automatically
                // or whether we leave that to developer to handle.
                return true;
            },
            uiShown: function() {
                // The widget is rendered.
                // Hide the loader.
                document.getElementById('loader').style.display = 'none';
            }
            },
            signInSuccessUrl: prev_URL,
            signInOptions: [
                firebase.auth.EmailAuthProvider.PROVIDER_ID,
                firebase.auth.PhoneAuthProvider.PROVIDER_ID
            ],
            };
    ui.start('#firebaseui-auth-container', uiConfig);
</script>

<div id="firebaseui-auth-container"></div>
<div id="loader">Loading...</div>