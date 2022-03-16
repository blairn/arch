<script>
	export let event;
	export let isCurrent;
</script>

<pre class:current={isCurrent}>{JSON.stringify(event, null, 2)}</pre>

<style>
	.current {
		background-color: blueviolet;
	}
</style>
