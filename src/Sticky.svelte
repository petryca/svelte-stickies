<script> 
    export let data;
    export let index;
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();
    const colors = ['#6eed2a', '#fcfa5d', '#f989d6', '#20dff8'];

    function updateColor(e) {
        data.c = (data.c + 1) % colors.length;
        e.target.style.backgroundColor = [data.c];
    }

    function keyPressed(e) {
        if (e.key === 'Enter') {
            e.preventDefault();
            updateColor(e);
        } else if (e.key === 'Tab') {
            e.preventDefault();
            dispatch('tab');
        } else if (e.key === 'Backspace' && data.s === '') {
            e.preventDefault();
            dispatch('delete', index);
        }
    }
</script>

<textarea style="background-color:{colors[data.c]};" maxlength="128" on:keydown={keyPressed} on:blur bind:value={data.s}></textarea>

<style>
    textarea {
        display: block;
        box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
        position: relative; 
        background-color: #6eed2a;
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
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4);
    }
</style>