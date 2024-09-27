<script lang="ts">
	import type { Filter } from '$lib/types';

	type Props = {
		filter?: Filter;
		onfilter?: (filter: Filter) => void;
	};

	let { filter = $bindable('all'), onfilter = () => {} }: Props = $props();

	$effect(() => onfilter(filter));
</script>

{#snippet filterButton(status: Filter)}
	<button
		class="btn toggle-btn"
		class:btn__primary={filter === status}
		onclick={() => (filter = status)}
	>
		<span>{status}</span>
	</button>
{/snippet}
<div class="filters btn-group stack-exception">
	{@render filterButton('all')}
	{@render filterButton('active')}
	{@render filterButton('completed')}
</div>
