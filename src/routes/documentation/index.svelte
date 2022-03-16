<script>
	import EventTree from './_lib/EventTree.svelte';
	import EventList from './_lib/EventList.svelte';
	let events = [
		{
			patient_id: 'blair',
			timestamp: '1973-05-16T02:24:00Z',
			gender: 'male',
			born: '1973-05-16T02:24:00Z'
		},
		{
			patient_id: 'blair',
			timestamp: '1973-05-16T02:26:00Z',
			gender: 'male',
			ward: 'neonatal',
			status: 'inpatient'
		},
		{
			patient_id: 'blair',
			timestamp: '1973-05-16T12:05:00Z',
			ward: null,
			status: 'discharged'
		},
		{
			patient_id: 'blair',
			timestamp: '1991-07-20T17:10:00Z',
			ward: 'ED',
			status: 'ED'
		},
		{
			patient_id: 'blair',
			timestamp: '1991-07-20T18:30:00Z',
			ward: 'neurology',
			status: 'inpatient'
		},
		{
			patient_id: 'john',
			timestamp: '1991-07-20T18:30:00Z',
			ward: 'neurology',
			status: 'inpatient'
		}
	];

	let rootNode = { key: 'root', children: [], dirty: false };

	const makeKey = (doc) => {
		const date = new Date(doc.timestamp);
		return [
			doc.patient_id,
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
		<EventTree node={rootNode} />
	</div>
	<div>
		<h1>Description</h1>
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
