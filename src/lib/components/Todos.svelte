<script lang="ts">
	import './styles.css';

	import type { Filter, Todo } from '$lib/types';
	import FilterButtons from '$lib/components/FilterButtons.svelte';

	type Props = {
		todos?: Todo[];
	};

	let { todos = $bindable([]) }: Props = $props();

	let total = $derived(todos.length);
	let completed = $derived(todos.filter((todo) => todo.completed).length);

	let name = $state('');

	let filter = $state<Filter>('all');

	let filtered = $derived.by(() => {
		if (filter === 'active') return todos.filter((todo) => !todo.completed);
		if (filter === 'completed') return todos.filter((todo) => todo.completed);
		return todos;
	});

	function onsubmit(e: SubmitEvent) {
		e.preventDefault();
		const id = todos.length ? Math.max(...todos.map((t) => t.id)) + 1 : 1;
		todos.push({ id, name, completed: false });
		name = '';
	}

	function remove(todo: Todo) {
		todos = todos.filter((t) => t.id !== todo.id);
	}

	$inspect({ name });
</script>

<!-- Todos.svelte -->
<div class="todoapp stack-large">
	<!-- NewTodo -->
	<form {onsubmit}>
		<h2 class="label-wrapper">
			<label for="todo-0" class="label__lg"> What needs to be done? </label>
		</h2>
		<input type="text" id="todo-0" autocomplete="off" class="input input__lg" bind:value={name} />
		<button type="submit" disabled={name === ''} class="btn btn__primary btn__lg"> Add </button>
	</form>

	<!-- Filter -->
	<FilterButtons {filter} onfilter={(value) => (filter = value)} />

	<!-- TodosStatus -->
	<h2 id="list-heading">{completed} out of {total} items completed</h2>

	<!-- Todos -->
	<ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
		{#each filtered as todo (todo.id)}
			<li class="todo">
				<div class="stack-small">
					<div class="c-cb">
						<input
							type="checkbox"
							id="todo{todo.id}"
							checked={todo.completed}
							onclick={() => (todo.completed = !todo.completed)}
						/>
						<label for="todo{todo.id}" class="todo-label">{todo.name}</label>
					</div>
					<div class="btn-group">
						<button type="button" class="btn">Edit</button>
						<button type="button" class="btn btn__danger" onclick={() => remove(todo)}>
							Delete
						</button>
					</div>
				</div>
			</li>
		{:else}
			Nothing to do!
		{/each}
	</ul>

	<hr />

	<!-- MoreActions -->
	<div class="btn-group">
		<button type="button" class="btn btn__primary">Check all</button>
		<button type="button" class="btn btn__primary">Remove completed</button>
	</div>
</div>
