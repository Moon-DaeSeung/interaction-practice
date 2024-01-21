<script lang="ts">
	import { onMount } from 'svelte';
	import CustomMouse from './custom-mouse/custom-mouse.svelte';

	const text1 = 'Yummy Tasty Delicious Useful Coding Yummy Yummmmy Yummmmmmmmmy yum';
	const text2 = 'Chicken Hamburger Pizza Salad Sushi Ramen Gimbab JJajangmyeon';
	const text3 = "Let's Dive Into This Tutorial Take It Easy! Don't Worry";
	const text4 = 'Pure Moral Conscientious Meritorious Worthy Exemplary Upright ';
	type Config = {
		el: HTMLElement;
		text: string;
		deg: number;
		count: number;
		direction: 1 | -1;
		color: string;
	};
	const banners = [
		{
			text: text1,
			deg: 2,
			count: 0,
			direction: 1,
			color: 'red'
		},
		{
			text: text2,
			deg: -2,
			count: 0,
			direction: -1,
			color: 'yellow'
		},
		{
			text: text3,
			deg: 2,
			count: 0,
			direction: 1,
			color: 'blue'
		},
		{
			text: text4,
			deg: -2,
			count: 0,
			direction: -1,
			color: 'purple'
		}
	] as unknown as Config[];

	onMount(() => {
		function animate() {
			for (let i = 0; i < banners.length; i++) {
				banners[i].count += 1;
				if (banners[i].count > banners[i].el.scrollWidth) {
					banners[i].count = 0;
				}
			}
			requestAnimationFrame(animate);
		}
		animate();
	});

	const handleScroll = (e: Event) => {
		banners.forEach((banner) => {
			banner.count += 15;
		});
	};
</script>

<svelte:window on:scroll={handleScroll} />
<div class="container">
	{#each banners as banner, i (banner.text)}
		<div
			bind:this={banners[i].el}
			class="banner"
			class:flex-end={banner.direction === 1}
			class:flex-start={banner.direction === -1}
			style="--color: {banner.color};--x: {banner.count *
				banner.direction}px;--deg: {banner.deg}deg"
		>
			<p>{banner.text}</p>
			<p>{banner.text}</p>
		</div>
	{/each}
</div>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Pacifico&display=swap');
	.container {
		font-family: 'Pacifico', cursive;
		overflow-x: hidden;
		font-size: 32px;
		height: 200vh;
		font-weight: 600;
		padding-block: 30px;
		display: flex;
		flex-direction: column;
		gap: 20px;
	}
	.banner {
		display: flex;
		word-spacing: 10px;
		transform: rotate(var(--deg, 0deg));
	}
	.banner > p {
		word-break: keep-all;
		white-space: nowrap;
		background-color: var(--color);
		word-spacing: 50px;
		transform: translate3d(var(--x, 400px), 0px, 0px);
	}
	.flex-start {
		justify-content: flex-start;
	}
	.flex-end {
		justify-content: flex-end;
	}
</style>
