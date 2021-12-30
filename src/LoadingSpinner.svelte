<div id="loadingindicatorcontainer">
<div id="loadingindicator"></div>
</div>

<style>
    #loadingindicatorcontainer {
        width: 100vw;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 6em 0;
    }
    #loadingindicator {
        border: 16px solid #fCfCfC; /* Light grey */
        border-top: 16px solid #EDEDED; /* Blue */
        border-radius: 50%;
        width: 120px;
        height: 120px;
        animation: spin 2s linear infinite;
    }
    @keyframes spin {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
        }
</style>
