<script>
import { onMount } from 'svelte';
import Sticky from './Sticky.svelte';
import Button from './Button.svelte';

let stickies = [{
	s: ''
}];

onMount(async () => {
	if (window.localStorage.getItem('stickies')) {
		stickies = JSON.parse(window.localStorage.getItem('stickies'));
		if (stickies.length == 1 && stickies[0].s === '') {
			document.querySelector('textarea:first-of-type').focus();
		}
	}
	window.onbeforeunload = () => {
		for (let i = 0; i < stickies.length; i++) { 
			if (stickies[i].s === '') {
				stickies.splice(i, 1);
			}
		}
		saveStickies();
	}
});

function  saveStickies() {
	console.log(stickies);
	const json = JSON.stringify(stickies);
	window.localStorage.setItem('stickies', json);
}

function deleteSticky(e) {
	if (stickies.length > 1) {
		document.querySelector( ':focus' ).blur();
		stickies.splice(e.detail, 1);
		stickies = stickies;
		saveStickies();
	}
}

function unshiftSticky() {
	stickies.unshift({s: ''});
	stickies = stickies;
	document.querySelector('textarea:first-of-type').focus();
}

function keyControl() {
	const firstEl = document.querySelector('textarea:first-of-type');
    const lastEl = document.querySelector('textarea:last-of-type');
	const focusedEl = document.activeElement;
	
	if (focusedEl !== lastEl) {
		focusedEl.nextElementSibling.focus();
	} else {
		firstEl.focus();
	}
}

</script>

<main>
	<Button on:click={unshiftSticky} />
	{#each stickies as sticky, i}
		<Sticky data={sticky} index={i} on:keyenter={keyControl} on:delete={deleteSticky} on:blur={saveStickies} />
	{/each}
</main>