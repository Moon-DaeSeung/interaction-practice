<script lang="ts">
	import { onMount } from 'svelte';

	const texts = ['Comwit', 'Is', 'Awesome', 'Blog'];
	let index = 0;
	let time = Date.now();
	let morthTime = 1;
	let cooldownTime = 1;
	let cooldown = cooldownTime;
	let morph = 0;
	$: t = morph / morthTime;
	$: u = 1 - t;
	$: isMorphing = cooldown <= 0;

	onMount(() => {
		function animate() {
			requestAnimationFrame(animate);
			const oldTime = time;
			time = Date.now();
			const dt = (time - oldTime) / 1000;
			const shouldTextChange = cooldown > 0;

			cooldown -= dt;

			if (cooldown <= 0) {
				if (shouldTextChange) {
					index++;
				}
				morph += dt;
				cooldown = 0;
			}

			if (morph > morthTime) {
				morph = 0;
				cooldown = cooldownTime;
			}
		}

		animate();
	});
</script>

<div class="container">
	<span
		class:morphing={isMorphing}
		class="text1"
		style="--o: {Math.pow(u, 0.4)}; --b: {Math.min(8 / u - 8, 100)}px"
		>{texts[index % texts.length]}</span
	>
	<span
		class:morphing={isMorphing}
		class="text2"
		style="--o: {Math.pow(t, 0.4)}; --b: {Math.min(8 / t - 8, 100)}px"
		>{texts[(index + 1) % texts.length]}</span
	>
</div>

<!-- The SVG filter used to create the merging effect -->
<svg id="filters">
	<defs>
		<filter id="threshold">
			<!-- Basically just a threshold effect - pixels with a high enough opacity are set to full opacity, and all other pixels are set to completely transparent. -->
			<feColorMatrix
				in="SourceGraphic"
				type="matrix"
				values="1 0 0 0 0
									0 1 0 0 0
									0 0 1 0 0
									0 0 0 300 -150"
			/>
		</filter>
	</defs>
</svg>

<style>
	:global(html, body) {
		width: 100%;
		height: 100vh;
		padding: 0;
		margin: 0;
	}
	.container {
		width: 100%;
		height: 100vh;
		display: flex;
		justify-content: center;
		align-items: center;
		position: relative;
		filter: url(#threshold) blur(0.6px);
	}
	.container > span {
		position: absolute;
		font-size: 10rem;
		font-weight: bold;
		font-size: 80pt;
	}
	.morphing {
		opacity: var(--o) !important;
		filter: blur(var(--b)) !important;
	}
	.text1 {
		opacity: 0;
	}
	.text2 {
		opacity: 1;
	}
</style>
