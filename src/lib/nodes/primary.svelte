<svelte:options accessors />

<script lang="ts">
	export let position: { x: number; y: number };
</script>

<div style="--x: {position.x}px; --y: {position.y}px"></div>

<style>
	div {
		transform: translate(calc(var(--x) - 50%), calc(var(--y) - 50%));
		position: absolute;
		width: 50px;
		height: 50px;
		background-color: #444;
		border-radius: 500px;
		border: 5px solid #000;
	}
</style>
