<script>
	import EventTree from './_lib/EventTree.svelte';
	import EventList from './_lib/EventList.svelte';
	let events = [
		{"id":"NHI_11523434","timestamp":"2020-02-01T01:00:00Z","name":"Blair","dob":"1973-05-16T07:00:00Z","gender":"male","status":"inpatient","gotham_ward":"Ward_8"},
		{"timestamp":"2020-02-01T04:00:00Z","gotham_ward":"Ward_2","id":"NHI_11523434"},
		{"status":"transfer","gotham_ward":null,"id":"NHI_11523434","timestamp":"2020-02-01T04:45:00Z"},
		{"id":"NHI_12523674","timestamp":"2020-02-01T01:15:00Z","name":"Kath","dob":"1976-10-02T07:00:00Z","gender":"female","status":"inpatient","gotham_ward":"Ward_12"},
		{"timestamp":"2020-02-01T02:00:00Z","gotham_ward":"Ward_6","id":"NHI_12523674"},
		{"timestamp":"2020-02-01T03:15:00Z","gotham_ward":"Ward_12","id":"NHI_12523674"},
{
    "id": "NHI_11523434",
    "timestamp": "2020-02-01T06:02:24Z",
    "status": "inpatient",
    "arkam_ward": "assessment"
  },{
    "id": "NHI_11523434",
    "timestamp": "2020-02-01T16:45:00Z",
    "arkam_ward": "enterprise architect"
  },{
    "id": "NHI_11523434",
    "timestamp": "2020-02-08T09:45:00Z",
    "status": "outpatient",
    "arkam_ward": null
  }
];

	let shown = {}

	let show = (d) => shown = d

	let k=0;


	let rootNode = { key: 'root', children: [], dirty: false };

	const makeKey = (doc) => {
		const date = new Date(doc.timestamp);
		return [
			doc.id,
			date.getUTCFullYear(),
			date.getUTCMonth(),
			date.getUTCDate(),
			date.getUTCHours(),
			date
		];
	};

	const reduce = (docs) => Object.assign({}, ...docs); //shallow merge, so not what we do in practice.

	const addRecord = (node, keys, doc) => {
		if (keys.length == 0) {
			node.doc = doc;
			node.label = '';
		} else {
			const key = keys.shift();
			let children = node.children;
			let child = children.find((c) => c.key == key);
			if (!child) {
				child = { key, children: [] };
				children.push(child);
				node.children.sort((a, b) => a.timestamp - b.timestamp);
			}
			child.dirty = true;

			// filthy hack so date names are shown for clarity
			if (keys.length == 2) {
				const date = keys[1];
				const month = date.toLocaleString('default', { month: 'short' });
				node.label = month;
			}
			addRecord(child, keys, doc);
			node.doc = reduce(node.children.map((x) => x.doc));
		}
		k++
	};

	let current_event = undefined;

	const indexRecord = (node) => (doc) => {
		console.log('adding record', doc);
		addRecord(rootNode, makeKey(doc), doc);
		current_event = doc;
		rootNode = rootNode;
	};

	const index = indexRecord(rootNode);

	const start = async () => {
		let time = 0;
		for (const event of events) {
			console.log('running', event);
			time += 2000;
			setTimeout(index, time, event);
		}
	};
	start();
</script>

<svelte:head>
	<title>MedRecord Remix - how it works</title>
</svelte:head>

<div class="page">
	<div>
		<h1>Data</h1>
		<EventList {events} {current_event} />
	</div>
	<div class="main">
		<h1>Tree</h1>
		<EventTree {show} node={rootNode} />
	</div>
	<div>
		<h1>Description</h1>
		{#key k}
		<pre>{JSON.stringify(shown.doc,null,2)}</pre>
		{/key}
	</div>
</div>

<style>
	.page {
		display: grid;
		grid-template-columns: var(--grid-6);
	}
	.main {
		grid-column-start: 2;
		grid-column-end: 5;
	}
</style>
