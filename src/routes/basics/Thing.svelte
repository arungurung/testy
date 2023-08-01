<script lang="ts">
	import { onDestroy } from 'svelte';

	const emojis: { [name: string]: string } = {
		apple: '🍎',
		banana: '🍌',
		carrot: '🥕',
		doughnut: '🍩',
		egg: '🥚'
	};

	export let name: string;

	let emoji: string = emojis[name];

	// observe in the console the thing destroyed
	onDestroy(() => {
		console.log('thing destroyed: ' + name);
	});
</script>

<p>
	<span>The emoji is {name} is {emoji}</span>
</p>

<style>
	p {
		margin: 0.8em 0;
	}
	span {
		display: inline-block;
		padding: 0.2em 1em 0.3em;
		text-align: center;
		border-radius: 0.2em;
		color: #333333;
		background-color: #ffdfd3;
	}
</style>
