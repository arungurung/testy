<script lang="ts">
	import { tweened } from 'svelte/motion';
	// import Decrementer from './Decrementer.svelte';
	// import Incrementer from './Incrementer.svelte';
	// import Resetter from './Resetter.svelte';
	import { count, time, countObj, name, greeting } from './stores';
	import { cubicOut } from 'svelte/easing';

	const formatter = new Intl.DateTimeFormat('en', {
		hour12: true,
		hour: 'numeric',
		minute: '2-digit',
		second: '2-digit'
	});

	const progress = tweened(0, {
		duration: 400,
		easing: cubicOut
	});
</script>

<h1>{$greeting}</h1>
<input type="text" bind:value={$name} />
<button on:click={() => ($name += '!')}> Add exclamation mark! </button>

<!-- <h1>The count is {$count}</h1>
<Incrementer />
<Decrementer />
<Resetter /> -->

<h1>The time is {formatter.format($time)}</h1>

<h1>The count is {$countObj}</h1>
<button on:click={countObj.increment}>+</button>
<button on:click={countObj.decrement}>-</button>
<button on:click={countObj.reset}>reset</button>
