<script lang="ts">
	export let position: { x: number; y: number };
</script>

<div style="--x: {position.x}px; --y: {position.y}px"></div>

<style>
	div {
		position: absolute;
		transform: translate(calc(var(--x) - 50%), calc(var(--y) - 50%));
		width: 40px;
		height: 40px;
		background-color: #da4949;
		border-radius: 500px;
		border: 3px solid #8d0e0e;
	}
</style>
