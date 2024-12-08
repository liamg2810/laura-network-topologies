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
				{ conn: [0, 1], vlan: false },
				{ conn: [1, 2], vlan: false },
				{ conn: [2, 3], vlan: false },
				{ conn: [3, 4], vlan: false },
				{ conn: [4, 0], vlan: false },
			],
			description:
				"A ring network is a network topology in which each node is connected to two other nodes, so as to create a ring-like structure.",
		},
		{
			name: "Star",
			nodes: [
				{ x: 300, y: 200, type: "primary" },
				{ x: 150, y: 150, type: "node" },
				{ x: 450, y: 150, type: "node" },
				{ x: 200, y: 350, type: "node" },
				{ x: 400, y: 350, type: "node" },
				{ x: 300, y: 50, type: "node" },
			],
			connections: [
				{ conn: [0, 1], vlan: false },
				{ conn: [0, 2], vlan: false },
				{ conn: [0, 3], vlan: false },
				{ conn: [0, 4], vlan: false },
				{ conn: [0, 5], vlan: false },
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
				{ conn: [0, 1], vlan: false },
				{ conn: [0, 2], vlan: false },
				{ conn: [0, 3], vlan: false },
				{ conn: [1, 2], vlan: false },
				{ conn: [1, 3], vlan: false },
				{ conn: [2, 3], vlan: false },
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
				{ conn: [0, 1], vlan: false },
				{ conn: [1, 2], vlan: false },
				{ conn: [1, 3], vlan: false },
				{ conn: [3, 4], vlan: false },
				{ conn: [3, 5], vlan: false },
				{ conn: [5, 6], vlan: false },
				{ conn: [5, 7], vlan: false },
			],
			description:
				"A bus network is a network topology in which each node is connected to a single central cable, called the bus or backbone with terminators each end.",
		},
		{
			name: "Hybrid",
			nodes: [
				{ x: 100, y: 200, type: "terminator" },
				{ x: 200, y: 200, type: "bus" },
				{ x: 200, y: 150, type: "primary" },
				{ x: 150, y: 100, type: "node" },
				{ x: 250, y: 100, type: "node" },
				{ x: 300, y: 200, type: "bus" },
				{ x: 300, y: 250, type: "primary" },
				{ x: 250, y: 300, type: "node" },
				{ x: 350, y: 300, type: "node" },
				{ x: 400, y: 200, type: "bus" },
				{ x: 400, y: 150, type: "primary" },
				{ x: 350, y: 100, type: "node" },
				{ x: 450, y: 100, type: "node" },
				{ x: 500, y: 200, type: "terminator" },
			],

			connections: [
				{ conn: [0, 5], vlan: false },
				{ conn: [5, 13], vlan: false },
				{ conn: [1, 2], vlan: false },
				{ conn: [2, 3], vlan: false },
				{ conn: [2, 4], vlan: false },
				{ conn: [1, 5], vlan: false },
				{ conn: [5, 6], vlan: false },
				{ conn: [6, 7], vlan: false },
				{ conn: [6, 8], vlan: false },
				{ conn: [5, 9], vlan: false },
				{ conn: [9, 10], vlan: false },
				{ conn: [10, 11], vlan: false },
				{ conn: [10, 12], vlan: false },
				{ conn: [9, 13], vlan: false },
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
				{ conn: [0, 1], vlan: false },
				{ conn: [0, 1], vlan: false },
				{ conn: [0, 2], vlan: false },
				{ conn: [1, 3], vlan: false },
				{ conn: [1, 4], vlan: false },
				{ conn: [2, 5], vlan: false },
				{ conn: [2, 6], vlan: false },
			],
			description:
				"A tree network is a network topology in which each node is connected to a parent node, forming a hierarchical structure.",
		},
		{
			name: "VLAN",
			nodes: [
				{ x: 200, y: 100, type: "primary" },
				{ x: 150, y: 200, type: "node" },
				{ x: 250, y: 200, type: "node" },
				{ x: 400, y: 100, type: "primary" },
				{ x: 350, y: 200, type: "node" },
				{ x: 450, y: 200, type: "node" },
				{ x: 600, y: 100, type: "primary" },
				{ x: 550, y: 200, type: "node" },
				{ x: 650, y: 200, type: "node" },
			],
			connections: [
				{ conn: [0, 1], vlan: false },
				{ conn: [0, 2], vlan: false },
				{ conn: [3, 4], vlan: false },
				{ conn: [3, 5], vlan: false },
				{ conn: [6, 7], vlan: false },
				{ conn: [6, 8], vlan: false },
				{ conn: [0, 3], vlan: true },
				{ conn: [3, 6], vlan: true },
			],
			description:
				"A VLAN network is a network topology in which nodes are connected through virtual LANs, allowing for logical segmentation of networks. For example, a VLAN network can be used to connect multiple departments in a company, while keeping their data separate.",
		},
	];

	let selectedTopology = topologies[0];
</script>

<div class="selector">
	<div class="buttons">
		{#each topologies as { name, nodes, connections }, i}
			<button
				class:selected={selectedTopology === topologies[i]}
				on:click={() => (selectedTopology = topologies[i])}
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

{#each selectedTopology.connections as conn}
	<Connection
		from={selectedTopology.nodes[conn.conn[0]]}
		to={selectedTopology.nodes[conn.conn[1]]}
		vlan={conn.vlan}
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
		border-radius: 5px;
		border: none;
		padding: 10px;
		cursor: pointer;
		font-size: 1.5rem;
	}

	button:hover {
		background-color: #666;
	}

	.selected {
		background-color: #777;
	}

	.selected:hover {
		background-color: #777;
	}

	p {
		font-size: 1.2rem;
		text-align: center;
		color: white;
		margin: 0;
		padding: 10px;
	}
</style>
