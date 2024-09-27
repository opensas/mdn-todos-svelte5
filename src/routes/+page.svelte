<script lang="ts">
	import type { Todo } from '$lib/types';

	import Todos from '$lib/components/Todos.svelte';

	let todos: Todo[] = $state([
		{ id: 1, name: 'Create a Svelte starter app', completed: true },
		{ id: 2, name: 'Create your first component', completed: true },
		{ id: 3, name: 'Complete the rest of the tutorial', completed: false }
	]);
</script>

<Todos {todos} />
