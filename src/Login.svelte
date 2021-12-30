<script>
    import { getAuth, signInWithEmailAndPassword  } from "firebase/auth";
    let pwd = ''
    let username = ''
    let pwd_validation_msg = "8 or more characters, \
        contain 1 letter, 1 number, \
        and 1 of these symbols: @$!%*?#&";
    let pwd_validation_pattern = "(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&-_])[A-Za-z\d@$!%*?&-_]{8,}$";
    async function do_login(email, password){
        const auth = getAuth();
        signInWithEmailAndPassword(auth, email, password)
        .then((userCredential) => {
            // Signed in 
            const user = userCredential.user;
            // ...
        })
        .catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
            console.log(errorCode)
            console.log(errorMessage)
        });
    }
</script>
<div class="maincontainer">
    <h3>Log in</h3>
    <form>
        <ul>
            <li>
                <label for="email">Email: </label>
                <input type="email" id="email" bind:value={username} placeholder="radindividual@test.xyz">
            </li>
            <li>
                <label for="password">Password: </label>
                <input type="password" id="password" bind:value={pwd} placeholder="*********"
                    minlength=8
                    pattern={pwd_validation_pattern}
                    title={pwd_validation_msg}>
            </li>
            <li>
                <input type="button" value="Log in" on:click={do_login(username,pwd)}>
            </li>
        </ul>
    </form>
</div>

<style>
    input:invalid {
        border: red solid 3px;
    }
    form > ul {
        list-style-type: none;
    }
    form>ul>li {
        padding: 0.3em;
    }
</style>