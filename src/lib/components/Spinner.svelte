<script lang="ts">
	let rotation = 0;
	let selectedEmoji = '';
	const emojis = ['🍣', '🍕', '🥗', '🍔', '🍅', '🍫'];
	const numberOfEmojis = emojis.length;

	const spin = () => {
		let spins = Math.floor(Math.random() * 10) + 1; // Random spins between 1 and 10
		rotation += spins * 360; // Calculate the total rotation
		rotation = rotation % 360; // Normalize rotation to [0, 360)
		let index = Math.floor(rotation / (360 / numberOfEmojis)); // Find the index of the emoji
		selectedEmoji = emojis[index];
		setTimeout(() => alert(`The wheel landed on: ${selectedEmoji}`), 2000); // Alert after 2 seconds (adjust as needed to match animation duration)
	};
</script>

<div>
	<ul class="wheel" style="transform: rotate({rotation}deg);">
		{#each emojis as emoji, i (emoji)}
			<li style="transform: rotate({i * (360 / numberOfEmojis)}deg);">
				<span class="emoji" style="transform: rotate(-{i * (360 / numberOfEmojis)}deg);"
					>{emoji}</span
				>
			</li>
		{/each}
	</ul>
	<button on:click={spin}>Spin the wheel!</button>
</div>

<style>
	.wheel {
		list-style: none;
		position: relative;
		width: 200px;
		height: 200px;
		padding: 0;
		border-radius: 50%;
		transform-origin: 50% 50%;
		transition: transform 2s ease-in-out; /* Adjust as needed */
		background: conic-gradient(
			hsla(0deg, 100%, 50%, 0.5) 30deg,
			hsla(60deg, 100%, 50%, 0.5) 30deg,
			hsla(60deg, 100%, 50%, 0.5) 90deg,
			hsla(120deg, 100%, 50%, 0.5) 90deg,
			hsla(120deg, 100%, 50%, 0.5) 150deg,
			hsla(180deg, 100%, 50%, 0.5) 150deg,
			hsla(180deg, 100%, 50%, 0.5) 210deg,
			hsla(240deg, 100%, 50%, 0.5) 210deg,
			hsla(240deg, 100%, 50%, 0.5) 270deg,
			hsla(300deg, 100%, 50%, 0.5) 270deg,
			hsla(300deg, 100%, 50%, 0.5) 330deg,
			hsla(0deg, 100%, 50%, 0.5) 330deg
		);
	}

	.wheel li {
		position: absolute;
		width: 100%;
		height: 100%;
		line-height: 200px; /* Adjust to match .wheel height */
		text-align: center;
		transform-origin: 50% 50%;
	}

	.emoji {
		display: inline-block;
		transform-origin: 50% 50%;
	}
</style>
