<script>
  import './tree.css'
  import EventTreeNode from './EventTreeNode.svelte'
  export let node={}
</script>
<ul class="tree">
  {#each node.children as child}
    <EventTreeNode node={child} />
  {/each}
</ul>