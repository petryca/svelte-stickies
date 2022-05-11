<script>
import { onMount, tick } from 'svelte';
import Sticky from './Sticky.svelte';
import Button from './Button.svelte';

const options = {  year: 'numeric', month: 'numeric', day: 'numeric'};
let now = new Date().toLocaleDateString(undefined, options);

let stickies = [];

window.onbeforeunload = () => {
	for (let i = 0; i < stickies.length; i++) { 
		if (stickies[i].s === '') {
			stickies.splice(i, 1);
		}
	}
	saveStickies();
}

onMount(async () => {
	if (window.localStorage.getItem('stickies')) {
		stickies = JSON.parse(window.localStorage.getItem('stickies'));
		if (stickies.length == 1 && stickies[0].s === '') {
			document.querySelector('textarea:first-of-type').focus();
		}
	}
});

function  saveStickies() {
	const json = JSON.stringify(stickies);
	window.localStorage.setItem('stickies', json);
}

async function deleteSticky(e) {
	const previousEl = document.activeElement.previousElementSibling;
	stickies.splice(e.detail, 1);
	stickies = stickies;
	saveStickies();
	await tick();
	if (stickies.length && previousEl.nodeName == 'TEXTAREA') {	
		previousEl.focus();
		previousEl.setSelectionRange(previousEl.value.length, previousEl.value.length);
	}
}

async function unshiftSticky() {
	stickies.unshift({s: now + ' ', c: 0});
	stickies = stickies;
	await tick();
	let element = document.querySelector('textarea:first-of-type');
	element.focus();
	element.setSelectionRange(element.value.length, element.value.length);
}

async function tabControl() {
    const lastEl = document.querySelector('textarea:last-of-type');
	const focusedEl = document.activeElement;
	
	if (focusedEl === lastEl) {
		stickies.push({s: now + ' ', c: 0});
		stickies = stickies;
		await tick();
		let element = document.querySelector('textarea:last-of-type');
		element.focus();
		element.setSelectionRange(element.value.length, element.value.length);
		console.log(element);
	} else {
		let element = focusedEl.nextElementSibling;
		element.focus();
		element.setSelectionRange(element.value.length, element.value.length);
	}
}

</script>

<main>
	<Button on:click={unshiftSticky} />
	{#each stickies as data, i}
		<Sticky bind:data index={i} on:tab={tabControl} on:delete={deleteSticky} on:blur={saveStickies} />
	{/each}
</main>