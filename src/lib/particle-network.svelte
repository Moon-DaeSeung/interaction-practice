<script lang="ts">
	import { onMount } from 'svelte';
	import { randomBetween, distance } from './util';

	let containerEl: HTMLElement;
	let canvasEl: HTMLCanvasElement;

	onMount(() => {
		const ctx = canvasEl.getContext('2d')!;
		const particles: Particle[] = [];
		class Line {
			constructor(
				private from: { x: number; y: number },
				private to: { x: number; y: number },
				private opacity: number
			) {}
			draw() {
				ctx.beginPath();
				ctx.moveTo(this.from.x, this.from.y);
				ctx.lineTo(this.to.x, this.to.y);
				ctx.strokeStyle = `rgba(255, 215, 0, ${this.opacity})`;
				ctx.lineWidth = 1;

				ctx.stroke();
			}
		}

		class Particle {
			constructor(
				public x: number,
				public y: number,
				public radius: number,
				private v: { x: number; y: number }
			) {}

			draw() {
				ctx.beginPath();
				ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
				ctx.fillStyle = 'white';
				ctx.fill();
				ctx.closePath();
			}

			animate() {
				if (this.x < 0 || this.x > canvasEl.width || this.y < 0 || this.y > canvasEl.height) {
					this.x = randomBetween(0, canvasEl.width);
					this.y = randomBetween(0, canvasEl.height);
				}
				this.x += this.v.x;
				this.y += this.v.y;

				const others: { x: number; y: number }[] = mousePoint
					? particles.concat(mousePoint as any)
					: particles;

				others.forEach((particle) => {
					const d = distance(this.x, this.y, particle.x, particle.y);
					if (d < 100) {
						const line = new Line(
							{ x: this.x, y: this.y },
							{ x: particle.x, y: particle.y },
							1 - d / 100
						);
						line.draw();
					}
				});

				this.draw();
			}
		}

		for (let i = 0; i < 70; i++) {
			particles.push(
				new Particle(
					randomBetween(0, canvasEl.width),
					randomBetween(0, canvasEl.height),
					randomBetween(0.5, 2),
					{ x: randomBetween(-2, 2), y: randomBetween(-2, 2) }
				)
			);
		}

		function render() {
			ctx.clearRect(0, 0, canvasEl.width, canvasEl.height);
			particles.forEach((particle) => particle.animate());
			requestAnimationFrame(render);
		}

		render();
	});

	onMount(() => {
		const observer = new ResizeObserver(([entry]) => {
			canvasEl.width = entry.contentRect.width;
			canvasEl.height = entry.contentRect.height;
		});
		observer.observe(containerEl);
		return () => observer.disconnect();
	});
	let mousePoint: { x: number; y: number } | null = null;
	const handleMouseMove = (e: MouseEvent) => {
		mousePoint = { x: e.clientX - containerEl.offsetLeft, y: e.clientY - containerEl.offsetTop };
	};
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	on:mousemove={handleMouseMove}
	on:mouseleave={() => (mousePoint = null)}
	bind:this={containerEl}
	class="container"
>
	<canvas bind:this={canvasEl} />
</div>

<style>
	.container {
		width: 100%;
		height: calc(100vh - 24px);
		background-color: black;
	}
</style>
