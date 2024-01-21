<script lang="ts">
	import { onMount } from 'svelte';
	import blossom from './image.png';

	let canvasEl: HTMLCanvasElement;
	let container: HTMLElement;

	onMount(() => {
		const image = new Image();
		image.src = blossom;
		const ctx = canvasEl.getContext('2d')!;

		class Petal {
			x = Math.random() * canvasEl.width;
			y = Math.random() * 2 * canvasEl.height - canvasEl.height;
			w = Math.random() * 15 + 30;
			h = Math.random() * 15 + 15;
			xSpeed = 2 + 2 * Math.random();
			ySpeed = 1 + 3 * Math.random();
			flip = Math.random();
			flipSpeed = 0.1 * Math.random();

			draw() {
				if (this.x > canvasEl.width || this.y > canvasEl.height) {
					this.x = -this.w;
					this.y = Math.random() * 2 * canvasEl.height - canvasEl.height;
					this.flip = Math.random();
				}
				ctx.globalAlpha = this.w / 45 + 0.5;
				ctx.drawImage(
					image,
					this.x,
					this.y,
					this.w * (0.66 + Math.abs(Math.cos(this.flip)) / 3),
					this.h * (0.8 + Math.abs(Math.cos(this.flip)) / 2)
				);
			}

			animate() {
				this.x += this.xSpeed;
				this.y += this.ySpeed;
				this.draw();
				this.flip += this.flipSpeed;
			}
		}

		const petals = Array.from({ length: 100 }, () => new Petal());
		image.onload = () => {
			render();
		};

		function render() {
			ctx.clearRect(0, 0, canvasEl.width, canvasEl.height);
			petals.forEach((petal) => petal.animate());
			requestAnimationFrame(render);
		}
	});

	onMount(() => {
		const observer = new ResizeObserver(([entry]) => {
			canvasEl.width = entry.contentRect.width;
			canvasEl.height = entry.contentRect.height;
		});
		observer.observe(container);
		return () => observer.disconnect();
	});
</script>

<div class="container" bind:this={container}>
	<canvas bind:this={canvasEl} />
</div>

<style>
	.container {
		width: 100%;
		height: calc(100vh - 24px);
    background-color: darkblue;
	}
</style>
