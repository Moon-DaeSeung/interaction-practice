<script lang="ts">
</script>

<div class="container">
	<div class="clouds">
		<div class="center-cloud"></div>
		{#each Array(20) as _, i}
			<div class="cloud-{i}" />
		{/each}
	</div>
</div>

<style lang="scss">
	:global(html, body) {
		width: 100%;
		height: 100vh;
		padding: 0;
		margin: 0;
	}
	.container {
		width: 100%;
		height: 100vh;
		background-color: black;
		overflow: hidden;
		filter: blur(8px) contrast(8);
	}
	.clouds {
		position: relative;
		width: 100%;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		animation: rotate 10s infinite linear;
	}

	.center-cloud {
		position: absolute;
		background-color: white;
		width: 150px;
		height: 150px;
		border-radius: 50%;
	}

	@keyframes rotate {
		0% {
			transform: rotate(0deg);
		}
		100% {
			transform: rotate(360deg);
		}
	}

	@for $i from 0 through 20 {
		$initX: random(400) - 200;
		$initY: random(400) - 200;
		$actionX: random(400) - 200;
		$actionY: random(400) - 200;
		$circle: random(50);
		$time: random(10) + 3;
		@keyframes groom {
			50% {
				transform: translateX($actionX + px) translateY($actionY + px);
			}
		}

		.cloud-#{$i} {
			position: absolute;
			background-color: white;
			width: $circle + px;
			height: $circle + px;
			border-radius: 50%;
			transform: translate($initX + px, $initY + px); /*처음위치*/
			animation: groom $time + s infinite alternate;
		}
	}
</style>
