<script lang="ts">
	import { onMount } from 'svelte';

	export let color;
	export let size: number;

	let canvas: HTMLCanvasElement;
	let context: CanvasRenderingContext2D | null;
	let previous;

	function get_coords(e: PointerEvent & { currentTarget: EventTarget & HTMLCanvasElement }) {
		const { clientX, clientY } = e;
		const { left, top } = canvas.getBoundingClientRect();
		const x = clientX - left;
		const y = clientY - top;
		return { x, y };
	}

	onMount(() => {
		context = canvas.getContext('2d');

		function resize() {
			canvas.width = window.innerWidth;
			canvas.height = window.innerHeight;
		}

		window.addEventListener('resize', resize);
		resize();

		return () => {
			window.removeEventListener('resize', resize);
		};
	});
</script>

{#if context}
	<canvas
		bind:this={canvas}
		on:pointerdown={(e) => {
			const coords = get_coords(e);
			context.fillStyle = color;
			context.beginPath();
			context.arc(coords.x, coords.y, size / 2, 0, 2 * Math.PI);
		}}
	/>
{/if}
