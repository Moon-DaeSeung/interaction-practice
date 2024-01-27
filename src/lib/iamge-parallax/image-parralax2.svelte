<script lang="ts">
	import { clamp } from '$lib/util';

	let progress = 0;
	let track: HTMLElement;
	let mouseDownAt: number | null = null;
	let mousePoint: { x: number; y: number } | null = null;
	let clicked = false;

	function moveHorizontally() {
		track.animate(
			{ transform: `translate(${-progress * 100}%, -50%)` },
			{
				duration: 1200,
				fill: 'forwards'
			}
		);
	}

	function moveImagePosition() {
		const images = track.children;
		for (let image of images) {
			image.animate(
				{
					objectPosition: `${progress * 100}% 50%`
				},
				{
					duration: 1000,
					fill: 'forwards'
				}
			);
		}
	}

	function moveCursor(e: MouseEvent) {
		mousePoint = {
			x: e.clientX,
			y: e.clientY
		};
	}

	const handleMouseDown = (e: MouseEvent) => {
		mouseDownAt = e.clientX;
		clicked = true;
	};
	const handleMouseMove = (e: MouseEvent) => {
		moveCursor(e);

		if (mouseDownAt === null) return;
		const dx = (e.clientX - mouseDownAt) * 0.01;
    
		const total = window.innerWidth * 0.5;
		progress = clamp(progress + dx / total, 0, 1);
		if (progress === 0 || progress === 1) {
			mouseDownAt = e.clientX;
		}

		moveHorizontally();
		moveImagePosition();
	};
	const handleMouseUp = () => {
		mouseDownAt = null;
		clicked = false;
	};
</script>

<svelte:window
	on:mouseenter={(e) => (mousePoint = { x: e.clientX, y: e.clientY })}
	on:mouseleave={() => (mousePoint = null)}
	on:mousedown={handleMouseDown}
	on:mousemove={handleMouseMove}
	on:mouseup={handleMouseUp}
/>
<div bind:this={track} id="image-track" data-mouse-down-at="0" data-prev-percentage="0">
	<img
		class="image"
		src="https://images.unsplash.com/photo-1524781289445-ddf8f5695861?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1770&q=80"
		alt="풍경"
		draggable="false"
	/>
	<img
		class="image"
		src="https://images.unsplash.com/photo-1610194352361-4c81a6a8967e?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1674&q=80"
		alt="풍경"
		draggable="false"
	/>
	<img
		class="image"
		src="https://images.unsplash.com/photo-1618202133208-2907bebba9e1?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1770&q=80"
		alt="풍경"
		draggable="false"
	/>
	<img
		class="image"
		src="https://images.unsplash.com/photo-1495805442109-bf1cf975750b?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1770&q=80"
		alt="풍경"
		draggable="false"
	/>
	<img
		class="image"
		src="https://images.unsplash.com/photo-1548021682-1720ed403a5b?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1770&q=80"
		alt="풍경"
		draggable="false"
	/>
	<img
		class="image"
		src="https://images.unsplash.com/photo-1496753480864-3e588e0269b3?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2134&q=80"
		alt="풍경"
		draggable="false"
	/>
	<img
		class="image"
		src="https://images.unsplash.com/photo-1613346945084-35cccc812dd5?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1759&q=80"
		alt="풍경"
		draggable="false"
	/>
	<img
		class="image"
		src="https://images.unsplash.com/photo-1516681100942-77d8e7f9dd97?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1770&q=80"
		alt="풍경"
		draggable="false"
	/>
</div>

{#if mousePoint}
	<div class="cursor-position" style="left: {mousePoint.x}px; top: {mousePoint.y}px;">
		<div class:cursor-clicked={clicked} class="cursor"></div>
	</div>
{/if}

<style>
	:global(html, body) {
		padding: 0;
		margin: 0;
		width: 100vw;
		height: 100vh;
		overflow: hidden;
		cursor: none;
		background-color: white;
	}

	#image-track {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(0%, -50%);
		display: flex;
		gap: 4vmin;
	}

	#image-track > .image {
		width: 46vmin;
		height: 52vmin;
		object-fit: cover;
		object-position: 0% 50%;
		user-select: none;
	}

	.cursor-position {
		position: absolute;
		width: 60px;
		height: 60px;
		transform: translate(-50%, -50%);
		z-index: 9999;
		mix-blend-mode: difference;
	}

	.cursor-position .cursor {
		width: 100%;
		height: 100%;
		border-radius: 50%;
		border: 1px solid white;
		transition: transform 0.2s ease-out;
		transform: scale(1);
	}
	.cursor.cursor-clicked {
		transform: scale(0.8);
	}
</style>
