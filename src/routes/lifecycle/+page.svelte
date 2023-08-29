<script lang="ts">
	import { afterUpdate, beforeUpdate, onMount, tick } from 'svelte';
	import { paint } from './gradient';

	onMount(() => {
		const canvas = document.querySelector('canvas');
		const context = canvas!.getContext('2d');

		let frame = requestAnimationFrame(function loop(t) {
			frame = requestAnimationFrame(loop);
			paint(context!, t);
		});

		return () => {
			cancelAnimationFrame(frame);
		};
	});

	beforeUpdate(() => {});

	afterUpdate(() => {});

	async () => {
		await tick();
	};
</script>

<canvas width={32} height={32} />

<style>
	canvas {
		position: fixed;
		left: 0;
		top: 0;
		width: 40%;
		height: 40%;
		background-color: #666;
		mask: url(./svelte-logo-mask.svg) 50% 50% no-repeat;
		mask-size: 40vmin;
		-webkit-mask: url(./svelte-logo-mask.svg) 50% 50% no-repeat;
		-webkit-mask-size: 20vmin;
	}
</style>
