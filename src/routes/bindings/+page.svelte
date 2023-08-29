<script lang="ts">
	type QuestionType = {
		id: number;
		text: string;
	};

	let name = 'world';

	let a = 1;
	let b = 2;

	let yes = false;

	let questions: QuestionType[] = [
		{
			id: 1,
			text: 'Where did you go to school?'
		},
		{
			id: 2,
			text: "What's your name?"
		}
	];
	let selected: QuestionType;
	let answer = '';

	function handleSubmit() {
		alert(`answered question ${selected.id} (${selected.text}) with "${answer}"`);
	}

	let scoops = 1;
	let flavours: string[] = [];
	const formatter = new Intl.ListFormat('en', { style: 'long', type: 'conjunction' });

	let textFieldValue = `Some words are *italic*, some are **bold**\n\n-lists\n- are\n- cool`;
</script>

<input bind:value={name} type="text" name="input" />

<h1>Hello {name}!</h1>

<label>
	<input type="number" bind:value={a} min="0" max="10" />
	<input type="range" bind:value={a} min="0" max="10" />
</label>

<label>
	<input type="number" bind:value={b} min="0" max="10" />
	<input type="range" bind:value={b} min="0" max="10" />
</label>

<p>{a} + {b} = {a + b}</p>

<h2>Size</h2>
{#each [1, 2, 3] as number}
	<label>
		<input type="radio" name="scoops" bind:group={scoops} value={number} />
		{number}
		{number === 1 ? 'scoop' : 'scoops'}
	</label>
{/each}
<select multiple bind:value={flavours}>
	{#each ['cookies and cream', 'mint chocolate chips', 'raspberry'] as flavour}
		<option>{flavour}</option>
	{/each}
</select>
{#if flavours.length === 0}
	<p>Please select at least one flavour</p>
{:else if flavours.length > scoops}
	<p>Can't order more flavours than scoops</p>
{:else}
	<p>
		You ordered {scoops}
		{scoops === 1 ? 'scoop' : 'scoops'} of {formatter.format(flavours)}
	</p>
{/if}

<h2>Questions</h2>
<form on:submit|preventDefault={handleSubmit}>
	<select bind:value={selected} on:change={() => (answer = '')}>
		{#each questions as question}
			<option value={question}>{question.text}</option>
		{/each}
	</select>

	<input type="text" bind:value={answer} />
	<button type="submit" disabled={!answer}>Submit</button>
</form>
<p>selected question {selected ? selected.id : '[waiting...]'}</p>

<label>
	<input type="checkbox" bind:checked={yes} />
	Yes! I agree
</label>

{#if yes}
	<p>Thank you.</p>
{:else}
	<p>You must agree first.</p>
{/if}

<button disabled={!yes}>Agreed</button>

<div class="grid">
	input
	<textarea bind:value={textFieldValue} />
	output
	<div>{@html textFieldValue}</div>
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: 5em 1fr;
		grid-template-rows: 1fr 1fr;
		gap: 1em;
		height: 100%;
	}

	textarea {
		flex: 1;
		resize: none;
	}
</style>
