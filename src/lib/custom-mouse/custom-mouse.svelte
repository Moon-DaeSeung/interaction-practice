<script lang="ts">
	import { onMount } from 'svelte';
	import image from './image.jpg';
	let container: HTMLElement;
	let cursorX: number | null = null;
	let cursorY: number | null = null;
	let scale = 1;
	let text = '';
	let stage = '';

	const handleMouseMove = (e: MouseEvent) => {
		cursorX = e.pageX;
		cursorY = e.pageY;

		const target = e.target as HTMLElement;
		switch (target.getAttribute('data-cursor')) {
			case 'carousel':
				const half = window.innerWidth / 2;
				text = cursorX < half ? 'Prev' : 'Next';
				if (stage === 'carousel') return;
				stage = 'carousel';
				scale = 5;
				break;
			case 'link':
				if (stage === 'link') return;
				text = target.getAttribute('data-text') ?? '';
				stage = 'link';
				scale = 5;
				break;
			case 'image':
				if (stage === 'image') return;
				stage = 'image';
				text = '';
				scale = 1;
				break;
			default:
				text = '';
				stage = '';
				scale = 1;
				break;
		}
	};
	const handleMouseDown = () => {
		scale *= 0.8;
	};
	const handleMouseLeave = () => {
		cursorX = null;
		cursorY = null;
	};
	const handleMouseUp = () => {
		scale *= 1.25;
	};
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	on:mousedown={handleMouseDown}
	on:mousemove={handleMouseMove}
	on:mouseleave={handleMouseLeave}
	on:mouseup={handleMouseUp}
	class="container"
	bind:this={container}
>
	<div data-cursor="carousel" class="carousel">carousel</div>
	<div class="links">
		<p data-cursor="link" data-text="Email">tmdeoans@snu.ac.kr</p>
		<p data-cursor="link" data-text="Phone">010-2222-1111</p>
	</div>
	<img data-cursor="image" src={image} alt="background" />
	{#if cursorX != null && cursorY != null}
		<div
			class:cursor-text-mode={text !== ''}
			style="--x: {cursorX}; --y: {cursorY}; --s: {scale}"
			class="cursor"
		>
			<div>{text}</div>
		</div>
	{/if}
</div>

<style>
	.container {
		width: 100%;
		height: calc(100vh - 24px);
		background-color: white;
		display: flex;
		flex-direction: column;
		font-family: Georgia, 'Times New Roman', Times, serif;
		position: relative;
		cursor: none;
	}
	.carousel {
		flex-grow: 1;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.links {
		display: flex;
		gap: 20px;
		align-items: center;
		padding: 8px 16px;
		border-top: 1px solid black;
	}
	img {
		width: 100%;
		height: 50%;
	}
	.cursor {
		position: absolute;
		--w: 30px;
		--h: 30px;
		top: 0px;
		left: 0px;
		transform: translate3d(
			calc(var(--x) * 1px - var(--w) / 2 - 20px),
			calc(var(--y) * 1px - var(--h) / 2 - 20px),
			0
		);
		mix-blend-mode: difference;
		
		pointer-events: none;
	}
	.cursor > div {
		width: var(--w, 30px);
		height: var(--h, 30px);
		transform: scale(var(--s));
		background-color: white;
		border-radius: 50%;
		transition:
			transform 200ms ease-in-out,
			background-color 200ms ease-in-out;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 8px;
	}
	.cursor-text-mode {
		mix-blend-mode: initial;
	}
	.cursor-text-mode > div {
		background-color: #333;
		color: white;
	}
</style>
