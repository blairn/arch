<script>
  import './tree.css'
  export let node
</script>
<li>
  <span title={JSON.stringify(node.doc,null,2)}>{node.label??node.key}</span>
  {#if node.children.length !== 0}
    <ul>
      {#each node.children as child}
        <svelte:self node={child} />
      {/each}
    </ul>
  {/if}
</li>