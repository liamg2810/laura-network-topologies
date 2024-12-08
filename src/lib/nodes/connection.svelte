<script lang="ts">
	import { onMount, type SvelteComponent } from "svelte";

	export let from: { x: number; y: number };
	export let to: { x: number; y: number };
</script>

{#if from && to}
	<svg class="connection-line" xmlns="http://www.w3.org/2000/svg">
		<line
			x1={from.x}
			y1={from.y}
			x2={to.x}
			y2={to.y}
			stroke="white"
			stroke-dasharray="10,10"
			stroke-width="3"
		/>
	</svg>
{/if}

<style>
	.connection-line {
		position: absolute;
		width: 100%;
		height: 100%;
		pointer-events: none;
		z-index: -1;
	}
</style>
