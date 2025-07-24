<script lang="ts">
	import imageCategories from "../assets/imageCategories.json";

	let image_url = "https://picsum.photos/200/300?random=1";
	let imageName = "...";
	let imageDescription = "...";
	let isShaking = false;
	let isSlowing = false;
	let isPopping = false;
	let intensity = 2; // Default intensity
	let intensityLabel = "Medium";
	const intensityMap: { [key: number]: string } = {
		1: "Easy",
		2: "Medium",
		3: "Hard",
		4: "Very Hard",
	};

	function handleIntensityChange(event: Event) {
		const range = event.target as HTMLInputElement;
		intensity = parseInt(range.value, 10);
		intensityLabel = intensityMap[intensity] || "Medium";
	}

	type IntensityKey = "Easy" | "Medium" | "Hard" | "Very Hard";
	function pickRandomImage() {
		const selectedIntensity = intensityMap[intensity] as IntensityKey;
		const images = imageCategories[selectedIntensity];
		const finalImage = images[Math.floor(Math.random() * images.length)];

		let count = 0;
		const totalTicks = 10;
		const tickSpeed = 150;

		isShaking = true;

		imageName = "...";
		imageDescription = "...";

		const slotInterval = setInterval(() => {
			count++;

			// Slow down shaking before the final tick
			if (count === totalTicks - 2) {
				isShaking = false;
				isSlowing = true;
			}

			if (count >= totalTicks) {
				clearInterval(slotInterval);

				setTimeout(() => {
					image_url = finalImage.image_url;
					imageName = finalImage.name;
					imageDescription = finalImage.description;
					isShaking = false;
					isSlowing = false;
					isPopping = true;

					setTimeout(() => {
						isPopping = false;
					}, 500);
				}, 300); // Wait 300ms before showing the final image
			}
		}, tickSpeed);
	}
</script>

<main>
	<div class="container">
		<h1>Pick Your Difficulty</h1>

		<!-- <label for="intensity-range">Difficulty:</label> -->
		<input
			type="range"
			id="intensity-range"
			min="1"
			max="4"
			step="1"
			value={intensity}
			on:change={handleIntensityChange}
		/>
		<div id="intensity-label">{intensityLabel}</div>

		<img
			id="displayed-image"
			class:shaking={isShaking}
			class:slowing={isSlowing}
			class:pop={isPopping}
			src={image_url}
			alt="Random"
		/>
		<div id="image-info">
			<h2 id="image-name" class="slot-item">{imageName}</h2>
			<p id="image-description" class="slot-item">{imageDescription}</p>
		</div>
		<button id="pick-btn" on:click={pickRandomImage}>Roll</button>
	</div>
</main>

<style>
	main {
		display: flex;
		/* align-items: center; */
		/* justify-content: center; */
	}

	.container {
		max-width: 600px;
		width: 90%;
		margin: auto;
		background: white;
		border-radius: 20px;
		box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
		padding: 30px;
		text-align: center;
	}

	h1 {
		color: #e84141;
		font-size: 2.4rem;
		font-weight: bold;
		margin-bottom: 30px;
	}

	img {
		width: 100%;
		max-height: 320px;
		object-fit: contain;
		border-radius: 16px;
		box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
		margin-top: 20px;
		transition: transform 0.3s ease;
	}

	#pick-btn {
		background: linear-gradient(to right, #ff5e7e, #e84141);
		border: none;
		padding: 12px 30px;
		font-size: 1.1rem;
		font-weight: bold;
		color: white;
		border-radius: 50px;
		margin-top: 30px;
		box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
		transition:
			transform 0.2s,
			background 0.3s;
	}

	#pick-btn:hover {
		transform: translateY(-2px);
		background: linear-gradient(to right, #ff7e9b, #e84141);
	}

	#intensity-range {
		width: 100%;
		margin: 20px 0 10px;
		appearance: none;
		height: 6px;
		background: #ddd;
		border-radius: 3px;
		outline: none;
	}

	#intensity-range::-webkit-slider-thumb {
		appearance: none;
		width: 20px;
		height: 20px;
		background: #e84141;
		border-radius: 50%;
		cursor: pointer;
		transition: background 0.3s;
	}

	#intensity-range::-moz-range-thumb {
		width: 20px;
		height: 20px;
		background: #e84141;
		border: none;
		border-radius: 50%;
		cursor: pointer;
	}

	#intensity-label {
		font-weight: bold;
		font-size: 1.2rem;
		color: #e84141;
		margin-bottom: 20px;
	}

	#image-info {
		margin-top: 25px;
		text-align: left;
		padding: 0 10px;
	}

	#image-name {
		margin: 10px 0 5px;
		font-size: 1.4rem;
		font-weight: bold;
		color: #333;
	}

	#image-description {
		color: #666;
		line-height: 1.4;
		font-size: 1rem;
	}

	#image-name,
	#image-description {
		transition:
			transform 0.2s ease,
			opacity 0.2s ease;
		display: inline-block;
	}

	#displayed-image.shaking {
		animation: shake 0.2s infinite;
	}

	#displayed-image.slowing {
		animation: shake 0.4s infinite ease-out;
	}

	@keyframes shake {
		0% {
			transform: rotate(0deg) scale(1);
		}
		25% {
			transform: rotate(1.5deg) scale(1.02);
		}
		50% {
			transform: rotate(-1.5deg) scale(1.02);
		}
		75% {
			transform: rotate(1deg) scale(1.01);
		}
		100% {
			transform: rotate(0deg) scale(1);
		}
	}

	#displayed-image.pop {
		animation: popOut 0.5s ease-out;
	}

	@keyframes popOut {
		0% {
			transform: scale(1) rotate(0deg);
			filter: brightness(1);
		}
		50% {
			transform: scale(1.3) rotate(3deg);
			filter: brightness(1.2);
		}
		100% {
			transform: scale(1) rotate(0deg);
			filter: brightness(1);
		}
	}
</style>
