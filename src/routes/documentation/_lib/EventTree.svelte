<script>
	import './tree.css';
	import EventTreeNode from './EventTreeNode.svelte';
	export let node = {};
	export let show
</script>

<ul class="tree">
	{#each node.children as child}
		<EventTreeNode {show} node={child} />
	{/each}
</ul>
