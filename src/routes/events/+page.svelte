<script lang="ts">
	import BigRedButton from './BigRedButton.svelte';
	import Outer from './Outer.svelte';
	import horn from './horn.mp3';

	let m = { x: 0, y: 0 };
	let name = 'world';

	const handleMessage = (e: CustomEvent<any>) => {
		alert(e.detail.text);
	};

	const audio = new Audio();
	audio.src = horn;

	const handlePlay = () => {
		audio.play();
	};
</script>

<Outer on:message={handleMessage} />

<!-- data flows from top down in Svelte -->
<BigRedButton on:click={handlePlay} />

<p>
	modifiers
	<li>
		<code>preventDefault</code> — calls event.preventDefault() before running the handler. Useful for
		client-side form handling, for example.
	</li>
	<li>
		<code>stopPropagation</code> — calls event.stopPropagation(), preventing the event reaching the next
		element
	</li>
	<li>
		<code>passive</code> — improves scrolling performance on touch/wheel events (Svelte will add it automatically
		where it's safe to do so)
	</li>
	<li><code>nonpassive</code> — explicitly set passive: false</li>
	<li>
		<code>capture</code> — fires the handler during the capture phase instead of the bubbling phase (MDN
		docs)
	</li>
	<li><code>once</code> — remove the handler after the first time it runs</li>
	<li><code>self</code> — only trigger handler if event.target is the element itself</li>
	<li>
		<code>trusted</code> — only trigger handler if event.isTrusted is true. I.e. if the event is triggered
		by a user action.
	</li>
</p>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	on:mousemove={(e) => {
		m = { x: e.clientX, y: e.clientY };
	}}
>
	The mouse position is {m.x} x {m.y}
</div>

<button on:click|once={() => alert('clicked')}>Can be clicked only once</button>

<style>
	div {
		width: 100%;
		height: 100%;
		flex: 1;
		background-color: white;
	}
</style>
