<script>
    export let appname="";
    function logout() {
        return
    }
</script>


<div id="navbar">
    <span id="appname">{appname}</span>
    <button class="linkstyle" id="logoutlink" on:click="{logout}" href="#">Log out</button>
</div>

<style>

    #navbar {
        position:fixed;
        top:0;
        background-color: black;
        color: white;
        font-size: 0.9em;
        padding: 3px;
        width: 100%;
        height: 1.2em;
    }
    #appname {
        height: 1.2em;
        color:white;
    }
    #logoutlink {
        position: absolute;
        right: 0.2em;
    }
</style>