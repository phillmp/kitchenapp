<script context="module">
    export const eventStates={
        'pass':'messagebar_pass',
        'fail':'messagebar_fail',
        'warn':'messagebar_warn',
        'notify':'messagebar_notify',
    };
    export class MessageBarEvent{
        constructor(message, state, duration=5000) {
            this.message = message;
            this.state = state;
            this.duration=duration;
        }
    }
</script>

<script>
    export let mbevent;
    import {onMount} from 'svelte'
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    onMount( () => {
        setTimeout(function() {
        dispatch('delete', {			
            });
        }, mbevent.duration);
});	
</script>
<!-- on:load="{setTimeout( handleRemoval,mbevent.duration, i) }" -->

<div class="messagebar {mbevent.state}" >
    {mbevent.message}
</div>


<style>
    .messagebar{
        display: flex;
        align-items: center;
        justify-content: center;
        background-color:black;
        color:white;
    }
    .messagebar.messagebar_pass {
        background-color: darkgreen;
    }
    .messagebar.messagebar_fail {
        background-color: darkred;
    }
    .messagebar.messagebar_warn {
        background-color: yellow;
        color:black;
    }
</style>