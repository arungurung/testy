<script lang="ts">
	import { flip } from 'svelte/animate';
	import { send, receive } from './transition';

	export let store: any;
	export let done: boolean;
</script>

<ul class="todos">
	{#each $store.filter((todo) => todo.done === done) as todo (todo.id)}
		<li
			class="done"
			in:receive={{ key: todo.id }}
			out:send={{ key: todo.id }}
			animate:flip={{ duration: 200 }}
		>
			<label>
				<input
					type="checkbox"
					checked={todo.done}
					on:change={(e) => store.mark(todo, e.currentTarget.checked)}
				/>
				<span>{todo.description}</span>

				<button on:click={() => store.remove(todo)} aria-label="Remove" />
			</label>
		</li>
	{/each}
</ul>

<style>
	label {
		width: 100%;
		height: 100%;
		display: flex;
	}

	span {
		flex: 1;
	}

	button {
		background-image: url(./remove.svg);
	}

	ul {
		padding: 0;
	}

	li {
		position: relative;
		display: flex;
		align-items: center;
		padding: 0.5em 0.5em 0.5em 1em;
		margin: 0 0 0.5em 0;
		gap: 0.5em;
		border-radius: 5px;
		user-select: none;
		background: var(--bg-1);
		filter: drop-shadow(2px 3px 6px rgba(0, 0, 0, 0.1));
		transition: filter 0.2s, opacity 0.2s;
	}
</style>
