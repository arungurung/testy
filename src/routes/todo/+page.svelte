<script lang="ts">
	import TodoList from './TodoList.svelte';
	import { createTodoStore } from './todos';

	const todos = createTodoStore([
		{ done: false, description: 'write some docs' },
		{ done: true, description: 'start writing blog post' },
		{ done: true, description: 'buy some milk' }
	]);
</script>

<div class="board">
	<input
		placeholder="what needs to be done?"
		on:keydown={(e) => {
			if (e.key === 'Enter') {
				todos.add(e.currentTarget.value);
				e.currentTarget.value = '';
			}
		}}
	/>

	<div class="todo">
		<h2 class="glitch">Todo</h2>
		<TodoList store={todos} done={false} />
	</div>

	<div class="done">
		<h2>Done</h2>
		<TodoList store={todos} done={true} />
	</div>
</div>

<style>
	.board {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-column-gap: 1em;
		max-width: 36em;
		margin: 0 auto;
	}

	.board > input {
		font-size: 1.4em;
		grid-column: 1/3;
		padding: 0.5em;
		margin: 0 0 1rem 0;
	}

	h2 {
		font-size: 2em;
		font-weight: 200;
	}

	.glitch {
		position: relative;
		display: inline-block;
		animation: glitch 1s infinite;
	}

	@keyframes glitch {
		0% {
			transform: translate(0);
		}
		25% {
			transform: translate(-5px, 5px);
		}
		50% {
			transform: translate(5px, -5px);
		}
		75% {
			transform: translate(-5px, 5px);
		}
		100% {
			transform: translate(0);
		}
	}
</style>
