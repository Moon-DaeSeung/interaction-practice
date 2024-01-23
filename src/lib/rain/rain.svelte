<script lang="ts">
	import { onMount } from 'svelte';
	import { randomBetween } from '../util';
	let mouse: { x: number; y: number } | null = null;
	let width = 0;
	let height = 0;
	let containerEl: HTMLElement;
	let canvasEl: HTMLCanvasElement;
	let ctx: CanvasRenderingContext2D;

	let rains: Rain[] = [];
	let drops: Drop[] = [];
	let thunder: Thunder;

	class Rain {
		constructor(
			public x: number,
			public y: number,
			public v: { x: number; y: number }
		) {}

		draw() {
			ctx.beginPath();
			ctx.moveTo(this.x, this.y);
			ctx.lineTo(this.x + this.v.x * 2, this.y + this.v.y * 2);
			ctx.strokeStyle = '#fff';
			ctx.lineWidth = 1;
			ctx.stroke();
		}

		splash() {
			for (let i = 0; i < 3; ++i) {
				const drop = new Drop(this.x, height, {
					x: -this.v.x + randomBetween(-2, 2),
					y: -this.v.y + randomBetween(5, 10)
				});
				drops.push(drop);
			}
		}
		isMouseActive = false;

		previousVx = 0;
		animate() {
			if (this.y > height) {
				this.y = -20;
				this.x = randomBetween(-0.2 * width, 1.2 * width);
				this.splash();
			}
			if (this.isMouseActive !== (mouse != null)) {
				this.isMouseActive = mouse != null;
				if (!this.isMouseActive) {
					this.v.x = randomBetween(-1, 1);
					this.previousVx = this.v.x;
				}
			}
			if (this.isMouseActive) {
				this.v.x = this.previousVx + (mouse!.x - width / 2) / 150;
			}
			this.x += this.v.x;
			this.y += this.v.y;
			this.draw();
		}
	}
	class Drop {
		constructor(
			public x: number,
			public y: number,
			public v: { x: number; y: number },
			public g: number = 0.5
		) {}
		draw() {
			ctx.beginPath();
			ctx.arc(this.x, this.y, 1, 0, Math.PI * 2, false);
			ctx.fillStyle = '#fff';
			ctx.fill();
			ctx.closePath();
		}

		animate() {
			this.v.y += this.g;
			this.x += this.v.x;
			this.y += this.v.y;
			this.draw();
		}
	}
	class Thunder {
		opacity = 1;

		draw() {
			const gradient = ctx.createLinearGradient(0, 0, 0, height);
			gradient.addColorStop(0, `rgba(66,88,99, ${this.opacity})`);
			gradient.addColorStop(1, `rgba(18,23,27, ${this.opacity})`);
			ctx.fillStyle = gradient;
			ctx.fillRect(0, 0, width, height);
		}

		animate() {
			if (this.opacity < 0) {
				this.opacity = 0;
				return;
			}
			this.opacity -= 0.01;
			this.draw();
		}
	}

	function init() {
		rains = Array.from(
			{ length: (width * height) / 10000 },
			() =>
				new Rain(randomBetween(0, width), randomBetween(0, height), {
					x: randomBetween(-1, 1),
					y: randomBetween(13, 18)
				})
		);
	}

	onMount(() => {
		ctx = canvasEl.getContext('2d')!;
		thunder = new Thunder();

		function render() {
			ctx.clearRect(0, 0, width, height);
			rains.forEach((rain) => {
				rain.animate();
			});
			drops.forEach((drop, i) => {
				drop.animate();
				if (drop.y > height) {
					drops.splice(i, 1);
				}
			});
			if (Math.random() < 0.0001) {
				thunder.opacity = 1;
			}
			thunder.animate();
			requestAnimationFrame(render);
		}

		render();
	});

	onMount(() => {
		const observer = new ResizeObserver((entries) => {
			const rect = entries[0].contentRect;
			width = rect.width;
			height = rect.height;
			init();
		});
		observer.observe(containerEl);
		return () => observer.disconnect();
	});
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	on:mousemove={(e) => (mouse = { x: e.offsetX, y: e.offsetY })}
	on:mouseleave={() => (mouse = null)}
	bind:this={containerEl}
	class="container"
>
	<canvas {width} {height} bind:this={canvasEl} />
</div>

<style>
	.container {
		width: 100%;
		height: calc(100vh - 48px);
		background-image: linear-gradient(to bottom, #222b33, #000000);
	}

	canvas {
		width: 100%;
		height: 100%;
	}
</style>
