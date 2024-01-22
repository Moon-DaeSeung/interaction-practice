<div class="container">
	<svg
		class="moving-circle"
		width="220"
		height="220"
		viewBox="0 0 168 168"
		fill="none"
		xmlns="http://www.w3.org/2000/svg"
	>
		<path
			d="M163 84C163 127.63 127.63 163 84 163C40.3695 163 5 127.63 5 84C5 40.3695 40.3695 5 84 5C127.63 5 163 40.3695 163 84Z"
			stroke="red"
			stroke-width="1"
		/>
	</svg>
</div>

<style>
	.container {
		width: 100%;
		height: calc(100vh - 24px);
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.moving-circle {
		stroke-dasharray: 500;
		stroke-dashoffset: 500;
		animation: ani 5s linear infinite;
	}

	@keyframes ani {
		0% {
			stroke-dashoffset: 500;
		}
		50% {
			stroke-dashoffset: 0;
		}
		100% {
			stroke-dashoffset: -500;
		}
	}
</style>
