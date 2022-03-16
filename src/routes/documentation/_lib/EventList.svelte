<script>
	import Event from './Event.svelte';
	export let events;
	export let current_event;
</script>

<div>
	{#each events as event}
		<Event {event} isCurrent={current_event === event} />
	{/each}
</div>
