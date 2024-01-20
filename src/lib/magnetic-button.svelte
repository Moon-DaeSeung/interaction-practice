<script lang="ts">
	import gsap, { Elastic } from 'gsap';
	let x = 0,
		y = 0;
	const r = 200;
	let element: HTMLButtonElement;
	let isTransition = false;

	const handleMouseMove = ({ pageX, pageY }: MouseEvent) => {
		isTransition = true;
		const { offsetLeft, offsetTop, offsetWidth, offsetHeight } = element;
		const left = pageX - offsetLeft;
		const top = pageY - offsetTop;
		x = left - r / 2;
		y = top - r / 2;
		gsap.to(element, {
			duration: 0.5,
			'--x': `${x / 1.5}px`,
			'--y': `${y / 1.5}px`,
			ease: Elastic.easeOut
		});
	};

	const handleMouseLeave = () => {
		isTransition = false;
		gsap.to(element, {
			duration: 1.2,
			'--x': '0px',
			'--y': '0px',
			ease: Elastic.easeOut
		});
		x = 0;
		y = 0;
	};
</script>

<div class="container">
	<button
		bind:this={element}
		on:mouseleave={handleMouseLeave}
		on:mousemove={handleMouseMove}
		style="--w: {r}px; --h: {r}px; --rx: {-y / 400}; --ry: {x / 400}; --o: {Math.sqrt(
			x ** 2 + y ** 2
		) / 5}deg"
	>
		<div class:transition={isTransition} class="span" style="--x: {x / 4}px; --y: {y / 4}px;">
			GET IN TOUCH
		</div>
	</button>
</div>

<style>
	.container {
		width: 100%;
		height: calc(100vh - 24px);
		display: flex;
		align-items: center;
		justify-content: center;
	}

	button {
		border-radius: 9999px;
		background-color: purple;
		width: var(--w);
		height: var(--h);
		transform: translate3d(var(--x), var(--y), 0);
		font-size: 20px;
		color: white;
	}

	.span {
		transform: translate3d(var(--x), var(--y), 0) rotate3d(var(--rx), var(--ry), 0, var(--o));
	}
	.transition {
		transition: transform 0.3s ease-out;
	}
</style>
