<script>
  import Event from './Event.svelte'
  export let events
</script>

<div>
  {#each events as event}
    <Event event={event}/>
  {/each}
</div>