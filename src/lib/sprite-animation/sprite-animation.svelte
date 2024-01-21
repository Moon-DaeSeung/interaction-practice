<script lang="ts">
	import { onMount } from 'svelte';
	import image from './image.png';
	let count = 0;

	onMount(() => {
		const interval = setInterval(() => {
			count += 1;
		}, 100);

		return () => clearInterval(interval);
	});
</script>

<div class="container">
	<div
		style="--image: url({image}); --x: {-(count % 5) * 60}px; --y: {(-Math.floor(count / 5) % 2) *
			62}px"
		class="image"
	/>
</div>

<style>
	.container {
		width: 100%;
		height: calc(100vh - 24px);
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.image {
		width: 60px;
		height: 64px;
		background-repeat: no-repeat;
		background-image: var(--image);
		background-position: var(--x) var(--y);
		background-size: 300px 128px;
	}
</style>
