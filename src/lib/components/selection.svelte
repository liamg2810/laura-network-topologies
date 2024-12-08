<script lang="ts">
	import Connection from "$lib/nodes/connection.svelte";
	import Node from "$lib/nodes/node.svelte";
	import Primary from "$lib/nodes/primary.svelte";
	import Terminator from "$lib/nodes/terminator.svelte";
	import type { SvelteComponent } from "svelte";

	const topologies = [
		{
			name: "Ring",
			nodes: [
				{ x: 100, y: 200, type: "node" },
				{ x: 200, y: 100, type: "node" },
				{ x: 300, y: 200, type: "node" },
				{ x: 250, y: 300, type: "node" },
				{ x: 150, y: 300, type: "node" },
			],
			connections: [
				[0, 1],
				[1, 2],
				[2, 3],
				[3, 4],
				[4, 0],
			],
			description:
				"A ring network is a network topology in which each node is connected to two other nodes, so as to create a ring-like structure.",
		},
		{
			name: "Star",
			nodes: [
				{ x: 300, y: 300, type: "primary" },
				{ x: 150, y: 200, type: "node" },
				{ x: 450, y: 200, type: "node" },
				{ x: 200, y: 400, type: "node" },
				{ x: 400, y: 400, type: "node" },
				{ x: 300, y: 100, type: "node" },
			],
			connections: [
				[0, 1],
				[0, 2],
				[0, 3],
				[0, 4],
				[0, 5],
			],
			description:
				"A star network is a network topology in which each node is connected to a central node, like a star.",
		},
		{
			name: "Mesh",
			nodes: [
				{ x: 100, y: 100, type: "node" },
				{ x: 300, y: 100, type: "node" },
				{ x: 100, y: 300, type: "node" },
				{ x: 300, y: 300, type: "node" },
			],
			connections: [
				[0, 1],
				[0, 2],
				[0, 3],
				[1, 2],
				[1, 3],
				[2, 3],
			],
			description:
				"A mesh network is a network topology in which each node relays data for the network. All nodes cooperate in the distribution of data in the network.",
		},

		{
			name: "Bus",
			nodes: [
				{ x: 100, y: 200, type: "terminator" },
				{ x: 200, y: 200, type: "bus" },
				{ x: 200, y: 100, type: "node" },
				{ x: 300, y: 200, type: "bus" },
				{ x: 300, y: 100, type: "node" },
				{ x: 400, y: 200, type: "bus" },
				{ x: 400, y: 100, type: "node" },
				{ x: 500, y: 200, type: "terminator" },
			],
			connections: [
				[0, 1],
				[1, 2],
				[1, 3],
				[3, 4],
				[3, 5],
				[5, 6],
				[5, 7],
			],
			description:
				"A bus network is a network topology in which each node is connected to a single central cable, called the bus or backbone.",
		},
		{
			name: "Hybrid",
			nodes: [
				{ x: 100, y: 100, type: "node" },
				{ x: 200, y: 100, type: "node" },
				{ x: 300, y: 100, type: "node" },
				{ x: 150, y: 200, type: "primary" },
				{ x: 250, y: 200, type: "node" },
				{ x: 200, y: 300, type: "node" },
			],
			connections: [
				[0, 1],
				[1, 2],
				[1, 3],
				[3, 4],
				[4, 5],
			],
			description:
				"A hybrid network is a network topology that uses a combination of two or more topologies.",
		},
		{
			name: "Tree",
			nodes: [
				{ x: 300, y: 100, type: "primary" },
				{ x: 200, y: 200, type: "node" },
				{ x: 400, y: 200, type: "node" },
				{ x: 150, y: 300, type: "node" },
				{ x: 250, y: 300, type: "node" },
				{ x: 350, y: 300, type: "node" },
				{ x: 450, y: 300, type: "node" },
			],
			connections: [
				[0, 1],
				[0, 2],
				[1, 3],
				[1, 4],
				[2, 5],
				[2, 6],
			],
			description:
				"A tree network is a network topology in which each node is connected to a parent node, forming a hierarchical structure.",
		},
	];

	let selectedTopology = topologies[0];
</script>

<div class="selector">
	<div class="buttons">
		{#each topologies as { name, nodes, connections }, i}
			<button on:click={() => (selectedTopology = topologies[i])}
				>{name}</button
			>
		{/each}
	</div>
	<p>{selectedTopology.description}</p>
</div>

{#each selectedTopology.nodes as node, i}
	{#if node.type === "node"}
		<Node position={node} />
	{:else if node.type === "primary"}
		<Primary position={node} />
	{:else if node.type === "terminator"}
		<Terminator position={node} />
	{/if}
{/each}

{#each selectedTopology.connections as [from, to]}
	<Connection
		from={selectedTopology.nodes[from]}
		to={selectedTopology.nodes[to]}
	/>
{/each}

<style>
	.selector {
		position: absolute;
		background-color: rgba(0, 0, 0, 0.3);
		width: 100%;
		height: 30%;
		top: 70%;
		padding-top: 10px;
	}

	.buttons {
		width: 100%;
		display: flex;
		justify-content: center;
		gap: 10px;
	}

	button {
		background-color: #444;
		color: white;
		border: none;
		padding: 10px;
		cursor: pointer;
		font-size: 1.5rem;
	}

	button:hover {
		background-color: #666;
	}

	p {
		font-size: 1.2rem;
		text-align: center;
		color: white;
		margin: 0;
		padding: 10px;
	}
</style>
