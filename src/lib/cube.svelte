<script lang="ts">
</script>

<div class="area">
	<div class="container">
		<div class="cube">
			<div class="face top">Top</div>
			<div class="face bottom">Bottom</div>
			<div class="face left">Left</div>
			<div class="face right">Right</div>
			<div class="face front">Front</div>
			<div class="face back">Back</div>
		</div>
	</div>
</div>

<style>
	:global(html, body) {
		width: 100%;
		height: 100vh;
		margin: 0;
		padding: 0;
	}

	.area {
		width: 100vw;
		height: 100vh;
		justify-content: center;
		display: flex;
		align-items: center;
		background-color: #f5f5f5;
	}
	.container {
		width: 200px;
		height: 200px;
		perspective: 500px;
		margin: 100px;
	}

	.cube {
		position: relative;
		width: 200px;
		height: 200px;
		transform-style: preserve-3d;
		transform: rotate3d(1, 1, 0, 45deg);
		animation: rotate 5s infinite linear;
	}

	@keyframes rotate {
		from {
			transform: rotate3d(0, 0, 0, 0deg);
		}
		to {
			transform: rotate3d(1, 1, 0, 360deg);
		}
	}

	.face {
		width: 200px;
		height: 200px;
		background: skyblue;
		border: 2px solid black;
		position: absolute;
		opacity: 0.5;
		display: flex;
		align-items: center;
		justify-content: center;
		font-family: Arial, sans-serif;
		font-size: 2rem;
	}

	.face.front {
		transform: translateZ(100px);
	}
	.face.back {
		transform: translateZ(-100px) rotateY(180deg);
	}
	.face.right {
		transform: translateX(-100px) rotateY(-90deg);
	}
	.face.left {
		transform: translateX(100px) rotateY(90deg);
	}
	.face.top {
		transform: translateY(-100px) rotateX(90deg);
	}
	.face.bottom {
		transform: translateY(100px) rotateX(-90deg);
	}
</style>
