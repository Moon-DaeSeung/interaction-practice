<script lang="ts">
	import { clamp, lerp } from '$lib/util';
	import imageSources from './images';
	let scrollY = 0;
	let animatedScrollY = scrollY;
	type CardEl = { el: HTMLElement; child: HTMLElement };
	const cardEls: CardEl[] = Array.from({ length: imageSources.length }, () => ({}) as any);

	let requestAnimationId: number;
	function animateScroll() {
		animatedScrollY = lerp(animatedScrollY, scrollY, 0.05);
		requestAnimationId = requestAnimationFrame(animateScroll);
		if (animatedScrollY.toFixed(1) === scrollY.toFixed(1)) {
			cancelAnimationFrame(requestAnimationId);
		}
	}
	function parallax({ child, el }: CardEl) {
		const diff = child.offsetHeight - el.offsetHeight;
		const { top } = el.getBoundingClientRect();
		const progress = top / window.innerHeight;
		const y = diff * progress;
		child.style.transform = `translateY(-${y}px)`;
	}
	function handleScroll(e: WheelEvent) {
		scrollY += e.deltaY / 2;
		scrollY = clamp(scrollY, 0, Infinity);
		cancelAnimationFrame(requestAnimationId);
		animateScroll();
		cardEls.forEach(parallax);
	}

	let galleryEl: HTMLElement;
</script>

<svelte:window on:wheel={handleScroll} />

<main bind:this={galleryEl} class="gallery">
	<div style="--y: {animatedScrollY}px" class="gallery-track">
		{#each imageSources as src, i}
			<div bind:this={cardEls[i].el} class="card">
				<div bind:this={cardEls[i].child} class="card-image-wrapper">
					<img alt="card" {src} />
				</div>
			</div>
		{/each}
	</div>
</main>

<style>
	:global(html, body) {
		padding: 0;
		margin: 0;
		width: 100%;
		height: 100%;
	}

	.gallery {
		width: 100%;
	}

	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	.gallery-track {
		position: fixed;
		top: 0px;
		left: 0px;
		display: grid;
		height: 100vh;
		grid-template-columns: repeat(3, 1fr);
		gap: 0.25rem;
		padding: 0.25rem;
		will-change: transform;
		transform: translateY(calc(-1 * var(--y)));
	}

	.card {
		height: 400px;
		overflow: hidden;

		& .card-image-wrapper {
			height: 135%;
			will-change: transform;

			& img {
				width: 100%;
				height: 100%;
				object-fit: cover;
			}
		}
	}

	@media (width < 800px) {
		.gallery-track {
			grid-template-columns: repeat(2, 1fr);
		}
	}

	@media (width < 550px) {
		.gallery-track {
			grid-template-columns: repeat(1, 1fr);
		}
	}
</style>
