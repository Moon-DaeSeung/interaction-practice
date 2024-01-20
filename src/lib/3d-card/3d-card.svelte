<script lang="ts">
	import background from './background.jpeg';
	let cardEl: HTMLElement;
	let lightEl: HTMLElement;

	const handleMouseMove = (e: MouseEvent) => {
		const { offsetWidth, offsetHeight } = cardEl;
		const cx = e.offsetX - offsetWidth / 2;
		const cy = e.offsetY - offsetHeight / 2;

		cardEl.style.transform = `rotate3d(${cy / 100}, ${cx / 100}, 0, ${Math.sqrt(cx ** 2 + cy ** 2) / 10}deg)`;
		cardEl.style.boxShadow = `${-cx / 5}px ${-cy / 5}px 2px 2px rgba(0, 0, 0, 0.1)`;
		lightEl.style.backgroundImage = `radial-gradient(circle at ${e.offsetX}px ${e.offsetY}px, #00000040, #ffffff00, #ffffff99)`;
	};

	const handleMouseLeave = () => {
		lightEl.style.backgroundImage = '';
		cardEl.style.boxShadow = '';
		cardEl.style.transform = 'rotate3d(0, 0, 0, 0deg)';
	};
</script>

<div class="container">
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div
		style=" --image: url({background});
		 "
		on:mousemove={handleMouseMove}
		on:mouseleave={handleMouseLeave}
		bind:this={cardEl}
		class="card"
	>
		<div bind:this={lightEl} class="light" />
	</div>
</div>

<style>
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: calc(100vh - 24px);
		background-color: white;
	}

	.card {
		width: 200px;
		height: 300px;
		/* transform: rotate3d(var(--rx), var(--ry), 0, var(--deg)); */
		transition: transform 0.2s ease-out;
		border-radius: 9px;
		background-image: var(--image);
		background-size: 100% 100%;
		background-repeat: no-repeat;
		/* box-shadow: var(--bx) var(--by) 2px 10px rgba(0, 0, 0, 0.2); */
		position: relative;
	}

	.light {
		position: absolute;
		width: 100%;
		height: 100%;
		/* background-image: radial-gradient(circle at var(--x) var(--y), #00000040, #ffffff00, #ffffff99); */
		border-radius: 9px;
	}
</style>
