<script> 
    export let data;
    export let index;
    import { createEventDispatcher } from 'svelte';
    
    const dispatch = createEventDispatcher();

    function keyPressed(e) {
        if ( e.key === 'Enter' || e.key === 'Tab') {
            e.preventDefault();
            dispatch('keyenter');
        } else if (e.key === 'Backspace' && data.s === '') {
            dispatch('delete', index);
        }
    }
</script>

<textarea maxlength="128" on:keydown={keyPressed} on:blur bind:value={data.s}></textarea>

<style>
    textarea {
        display: block;
        background-color: #fff6e1;
        padding: 1rem;
        flex-basis: 20%;
        transition: all 0.3s;
        border-width: 0;
        resize: none;
        outline: none;
        font: inherit;
        color: inherit;
        line-height: inherit;
    }
    textarea:focus {
        background-color: #fffcf7;
    }
</style>