<script lang="ts">
</script>

<div class="container">
	<span class="left">COMWIT</span>
	<span class="right">WORKS</span>
	<span class="top">PRODUCT</span>
	<span class="bottom">BLOG</span>
	<span class="center">CONTACT</span>
</div>

<style>
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: calc(100vh - 24px);
		background-color: #2f2f2f;
		font-family: 'Black Han Sans', sans-serif;
	}
	span {
		font-size: 3rem;
		font-weight: 700;
		color: transparent;
		cursor: pointer;
		-webkit-text-stroke: 1px white;
		-webkit-background-clip: text;
		background-image: linear-gradient(0deg, #f9f7f1, #f9f7f1);
		background-repeat: no-repeat;
		transition: background-size 0.2s ease-in-out;
	}
	span:hover {
		background-size: 100% 100%;
	}

	.left, .right {
		background-size: 0% 100%;
	}
	.top, .bottom {
		background-size: 100% 0%;
	}
	.left {
		background-position: 0% 0%;
	}
	.right {
		background-position: 100% 0%;
	}
	.top {
		background-position: 0% 0%;
	}
	.bottom {
		background-position: 0% 100%;
	}
	.center {
		background-position: 50% 50%;
		background-size: 0% 0%;
	}
</style>
