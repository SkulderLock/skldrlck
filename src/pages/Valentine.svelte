<script lang="ts">
	let noBtn: HTMLButtonElement;
	let yesClicked = false;
	let bounceIndex = 0;
	let headingText = "Will you be my Valentine?";

	const messages = ["( ノ ^o^)ノ", "( ノ・・)ノ"];
	let bounceInterval: number;

	function handleYesClick() {
		yesClicked = true;

		bounceInterval = setInterval(() => {
			bounceIndex = (bounceIndex + 1) % messages.length;
		}, 500);

		setTimeout(() => {
			clearInterval(bounceInterval);
			headingText = "Yay! Thank you for being my Valentine!";
		}, 5000);
	}

	function handleNoClick() {
		// When "No" is clicked, move the button to a random position
		// Get viewport dimensions
		const maxX = window.innerWidth - noBtn.offsetWidth;
		const maxY = window.innerHeight - noBtn.offsetHeight;

		// Get random x,y
		const randomX = Math.floor(Math.random() * maxX);
		const randomY = Math.floor(Math.random() * maxY);

		noBtn.style.position = "absolute";

		noBtn.style.left = `${randomX}px`;
		noBtn.style.top = `${randomY}px`;

		headingText += "?";
	}

	$: if (yesClicked) {
		headingText = messages[bounceIndex];
	}
</script>

<main>
	<!-- Cute outlined heart (inline SVG) -->
	<svg
		class="heart-icon"
		width="64"
		height="64"
		viewBox="0 0 24 24"
		fill="none"
		stroke="#ec7a7a"
		stroke-width="2"
		stroke-linecap="round"
		stroke-linejoin="round"
	>
		<path
			d="M12 21s-8-4.435-8-11c0-2.336 1.842-4.5 4.5-4.5 
             2.286 0 3.776 1.5 3.776 1.5s1.49-1.5 3.776-1.5C18.158 5.5 
             20 7.664 20 10c0 6.565-8 11-8 11z"
		/>
	</svg>

	<h1>{headingText}</h1>

	{#if !yesClicked}
		<button id="yes" on:click={handleYesClick}>Yes</button>
		<button id="no" bind:this={noBtn} on:click={handleNoClick}>No</button>
	{/if}
</main>

<style>
	/* Style the heart icon */
	.heart-icon {
		width: 64px;
		height: 64px;
		/* Already has stroke set in the SVG, but you can adjust here if you like */
		margin-bottom: 16px; /* Space between heart and heading */
		animation: pulse 2s linear infinite alternate;
	}

	@keyframes pulse {
		from {
			opacity: 1;
		}
		to {
			opacity: 0.5;
		}
	}

	/* Center and style heading */
	h1 {
		text-align: center;
		color: #e84141; /* Red text color */
		font-size: 2rem;
		margin-bottom: 24px; /* Space below heading before buttons */
	}

	/* Basic button styling (tweak as you like) */
	button {
		font-size: 16px;
		font-weight: bold;
		border: none;
		border-radius: 30px;
		padding: 15px 40px;
		cursor: pointer;
		margin: 8px;
		/* display: inline-block; */
	}

	#yes {
		background-color: #e84141; /* Red button */
		color: #fff;
	}

	#no {
		background-color: #d3d8dd; /* Gray button */
		color: #000;
		transition:
			top 0.2s,
			left 0.2s;
		position: absolute;
		top: 70%;
	}

	main {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

		/* Full viewport height */
		min-height: 100vh;
		background-color: #fdebf3; /* Light pink background */
		font-family: Arial, sans-serif;
		margin: 0;
		position: relative;
	}
</style>
