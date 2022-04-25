<script>
import gotham from './gotham.json'
import arkam from './arkam.json'

const make_diff = (o1,o2) => Object.keys(o2).reduce((diff, key) => {
  if (o1[key] === o2[key]) return diff
  return {
    ...diff,
    [key]: o2[key]
  }
}, {})

const make_data_diff = (o1, o2) => {
  const diff = make_diff(o1,o2)
  diff.id = diff.id??o1.id
  diff.timestamp = diff.timestamp??o1.timestamp
  if (Object.keys(diff).length > 2) return diff
  return {}
}

let current = {}
let results = []
for (let i of gotham) {
  const d = make_data_diff(current, i)
  if (Object.keys(d).length != 0) {
    results.push(d)
  }
  current = i
}

</script>

{#each results as result}
  {JSON.stringify(result)}<br>
{/each}
<!-- {arkam}

{gotham} -->