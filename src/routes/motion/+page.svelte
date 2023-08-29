<script lang="ts">
	import { cubicOut, elasticOut } from 'svelte/easing';
	import { spring, tweened } from 'svelte/motion';
	import { fade, fly } from 'svelte/transition';

	const progress = tweened(0, {
		duration: 400,
		easing: cubicOut
	});

	let coords = spring(
		{ x: 50, y: 50 },
		{
			stiffness: 0.1,
			damping: 0.25
		}
	);
	let size = spring(10);
	let status = 'waiting...';

	let visible = true;

	function spin(node: HTMLDivElement, { duration }: { duration: number }) {
		return {
			duration,
			css: (t: number) => {
				const eased = elasticOut(t);
				return `
					transform: scale(${eased}) rotate(${eased * 1080}deg);
					color: hsl(
						${Math.trunc(t * 360)},
						${Math.min(100, 1000 * (1 - t))}%,
						${Math.min(50, 500 * (1 - t))}%
					);`;
			}
		};
	}

	function typewriter(node: HTMLDivElement, { speed = 1 }) {
		const valid = node.childNodes.length === 1 && node.childNodes[0].nodeType === Node.TEXT_NODE;
		if (!valid) {
			throw new Error(`This transition only works on elements with a single text node child`);
		}

		const text = node.textContent;
		const duration = text?.length! / (speed * 0.01);

		return {
			duration,
			tick: (t: number) => {
				const i = Math.trunc(text?.length! * t);
				node.textContent = text?.slice(0, i)!;
			}
		};
	}

	// function fade(
	// 	node,
	// 	{ delay = 0, duration = 400 }
	// ): { delay: number; duration: number; css: (t: any) => string } {
	// 	const o = +getComputedStyle(node).opacity;
	// 	return {
	// 		delay,
	// 		duration,
	// 		css: (t) => `opacity: ${t * o}`
	// 	};
	// }
</script>

<progress value={$progress} />
<button on:click={() => progress.set(0)}>0%</button>
<button on:click={() => progress.set(0.25)}>25%</button>
<button on:click={() => progress.set(0.5)}>50%</button>
<button on:click={() => progress.set(0.75)}>75%</button>
<button on:click={() => progress.set(1)}>100%</button>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<!-- <svg
	on:mousemove={(e) => {
		coords.set({ x: e.clientX, y: e.clientY });
	}}
	on:mousedown={() => size.set(30)}
	on:mouseup={() => size.set(10)}
>
	<circle cx={$coords.x} cy={$coords.y} r={$size} />
</svg>

<div class="controls">
	<label>
		<h3>stiffness ({coords.stiffness})</h3>
		<input bind:value={coords.stiffness} type="range" min="0.01" max="1" step="0.01" />
	</label>
	<label>
		<h3>damping ({coords.damping})</h3>
		<input type="range" bind:value={coords.damping} min="0.01" max="1" step="0.01" />
	</label>
</div> -->

<label>
	<input type="checkbox" bind:checked={visible} />
	visible
</label>

<p>Status: {status}</p>

{#if visible}
	<!-- <p transition:fade>Fades in and out</p>
	<p transition:fly={{ y: 200, duration: 2000 }}>Flies in and out</p>
	<p in:fly={{ y: 200, duration: 2000 }} out:fade>Flies in and out</p> -->

	<div class="centered" in:spin={{ duration: 8000 }} out:fade><span>spin transitions!</span></div>

	<p
		transition:typewriter
		on:introstart={() => (status = 'intro started')}
		on:outrostart={() => (status = 'outro started')}
		on:introend={() => (status = 'intro ended')}
		on:outroend={() => (status = 'outro ended')}
	>
		The quick brown fox jumps over the lazy dog
	</p>
{/if}

<style>
	svg {
		position: absolute;
		width: 100%;
		height: 100%;
		left: 0;
		top: 0;
	}

	circle {
		fill: #ff3e00;
	}

	.controls {
		position: absolute;
		top: 1em;
		right: 1em;
		width: 200px;
		user-select: none;
	}

	.controls input {
		width: 100%;
	}

	.centered {
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
	}

	span {
		position: absolute;
		transform: translate(-50%, -50%);
		font-size: 4em;
	}
</style>
