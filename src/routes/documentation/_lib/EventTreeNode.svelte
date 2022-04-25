<script>
	import './tree.css';
	export let node;
	import { fade, fly } from 'svelte/transition';
	export let show
</script>

<li transition:fade>
	<span on:mouseover={() => show(node)} title={JSON.stringify(node.doc, null, 2)}>{node.label ?? node.key}</span>
	{#if node.children.length !== 0}
		<ul>
			{#each node.children as child}
				<svelte:self {show} node={child} />
			{/each}
		</ul>
	{/if}
</li>
