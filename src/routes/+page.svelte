<script>
	import Counter from './Counter.svelte';
	import Sum from './Sum.svelte';

	let name = 'Arun';

	const toggleName = () => {
		if (name === 'World') {
			name = 'Arun';
		} else {
			name = 'World';
		}
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

	$: sum = numbers.reduce((total, currentNumber) => total + currentNumber, 0);
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="app using Svelte" />
</svelte:head>

<section>
	<h1>Hello {name.toUpperCase()}!</h1>
	<button on:click={toggleName} type="button">Toggle name</button>
	<Counter />
	<button on:click={increment} type="button"
		>Clicked {count} {count === 1 ? 'time' : 'times'}</button
	>
	<p>{count} doubled is {doubled}</p>

	<p>{numbers.join(' + ')} = {sum}</p>
	<Sum {sum} />
	<button type="button" on:click={addNumber}>Add a number</button>
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
