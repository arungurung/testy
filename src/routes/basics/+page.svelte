<script>
	import Counter from './Counter.svelte';
	import PackageInfo from './PackageInfo.svelte';
	import User from './User.svelte';
	import Cats from './Cats.svelte';
	import Thing from './Thing.svelte';
	import Sum from './Sum.svelte';

	let name = 'Arun';
	const toggleName = () => {
		if (name === 'World') {
			name = 'Arun';
		} else {
			name = 'World';
		}
	};

	const pkg = {
		name: 'svelte',
		version: 3,
		speed: 'blazing',
		website: 'https://svelte.dev'
	};

	let count = 0;
	$: {
		console.log(`the count is ${count}`);
		console.log('this will also be logged when count changes');
	}

	$: if (count >= 10) {
		alert('count is dangerously high');
		count = 0;
	}

	const increment = () => {
		count++;
	};

	$: doubled = count * 2;

	let numbers = [1, 2, 3, 4, 5, 6, 7];
	function addNumber() {
		numbers = [...numbers, numbers.length + 1];
		// numbers.push(numbers.length + 1);
		// numbers = numbers
	}

	let things = [
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' }
	];
	function removeThing() {
		things = things.slice(1);
	}

	$: sum = numbers.reduce((total, currentNumber) => total + currentNumber, 0);

	async function getRandomNumber() {
		const res =
			await fetch(`http://www.randomnumberapi.com/api/v1.0/randomredditnumber?min=100&max=1000&count=1
`);
		const text = await res.text();

		if (res.ok) {
			return text;
		} else {
			throw new Error(text);
		}
	}
	let promise = getRandomNumber();

	function randomButtonClick() {
		promise = getRandomNumber();
	}
</script>

<svelte:head>
	<title>Basics</title>
	<meta name="description" content="svelte basics" />
</svelte:head>

<section>
	<h1>Hello {name.toUpperCase()}!</h1>
	<button on:click={toggleName} type="button">Toggle name</button>

	{#await promise}
		<p>...waiting</p>
	{:then number}
		<p>The number is {number}</p>
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}

	<button type="button" on:click={randomButtonClick}>Generate a random number</button>

	<Counter />
	<button on:click={increment} type="button"
		>Clicked {count} {count === 1 ? 'time' : 'times'}</button
	>
	<p>{count} doubled is {doubled}</p>

	<p>{numbers.join(' + ')} = {sum}</p>
	<Sum {sum} />
	<button type="button" on:click={addNumber}>Add a number</button>

	<PackageInfo {...pkg} />

	<User />

	<Cats />

	<button on:click={removeThing}>Remove first thing</button>
	{#each things as thing (thing.id)}
		<Thing name={thing.name} />
	{/each}
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}
</style>
