<script lang="ts">
	const text = 'Comwit';
	let count = 0;

	let skew = 0;
	let scale = 1;
	let t1 = 0,
		t2 = 0;
	let b1 = 100,
		b2 = 100;

	setInterval(() => {
		count++;
		skew = Math.random() * 20 - 10;
		t1 = Math.random() * 100;
		t2 = Math.random() * 100;
		b1 = Math.random() * 100;
		b2 = Math.random() * 100;
		scale = 1;

		if (count % 15 === 0) {
			skew = Math.random() * 40 - 20;
		}

		if (count % 30 === 0) {
			scale = Math.random() / 2 + 1;
		}
	}, 100);
</script>

<div class="container">
	<p
		style="
    --t1: {t1}%;
    --t2: {t2}%;
    --b1: {b1}%;
    --b2: {b2}%;
    --skew: {skew}deg;
    --scale: {scale};
    "
		data-text={text}
		class="outlined-text"
	>
		{text}
	</p>
</div>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Orbitron&display=swap');

	.container {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		height: calc(100vh - 24px);
		background-color: #1a1a1a;
	}

	p {
		font-family: 'Orbitron', sans-serif;
		font-size: 48px;
		color: #f1f1f1;
		overflow: hidden;
		position: relative;
		font-weight: 700;
		transform: skew(var(--skew)) scale(var(--scale));
	}
	p::after,
	p::before {
		content: attr(data-text);
		position: absolute;
		width: 100%;
		transition: clip-path 50ms ease-in;
		background-color: #1a1a1a;
	}

	p::before {
		left: 5px;
		text-shadow: 2px -2px #2a96d4;
		clip-path: polygon(0 var(--t1), 100% var(--t1), 100% var(--b1), 0 var(--b1));
	}

	p::after {
		left: -5px;
		text-shadow: -2px 2px red;
		clip-path: polygon(0 var(--t2), 100% var(--t2), 100% var(--b2), 0 var(--b2));
	}

	.outlined-text {
		-webkit-text-stroke: 1px white;
		/* -webkit-text-fill-color: transparent;
    text-fill-color: transparent; */
	}
</style>
