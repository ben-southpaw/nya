<script>
	import { onMount } from 'svelte';

	// State management
	export let fontSize = 50;
	export let fontWeight = 400;
	export let userText = '';

	let isDraggingSize = false;
	let isDraggingWeight = false;

	// Font size options
	const fontSizes = [8, 16, 24, 32, 48, 64, 80, 96, 120, 240, 320];
	const fontWeights = [100, 200, 300, 400, 500, 600, 700, 800, 900];

	// Show/hide dropdowns
	let showSizeDropdown = false;
	let showWeightDropdown = false;

	// Handle slider drag
	function handleMousedown(type) {
		if (type === 'size') isDraggingSize = true;
		if (type === 'weight') isDraggingWeight = true;
	}

	function handleMouseup() {
		isDraggingSize = false;
		isDraggingWeight = false;
	}

	function handleMousemove(e, type, min, max) {
		if (
			(type === 'size' && !isDraggingSize) ||
			(type === 'weight' && !isDraggingWeight)
		)
			return;

		const slider = document.getElementById(
			type === 'size' ? 'sizeSlider' : 'weightSlider'
		);
		const rect = slider.getBoundingClientRect();
		const percentage = Math.max(
			0,
			Math.min(1, (e.clientX - rect.left) / rect.width)
		);

		if (type === 'size') {
			fontSize = Math.round(min + (max - min) * percentage);
		} else {
			fontWeight = Math.round(min + (max - min) * percentage);
		}
	}

	// Close dropdowns when clicking outside
	function handleClickOutside(event) {
		if (!event.target.closest('.dropdown')) {
			showSizeDropdown = false;
			showWeightDropdown = false;
		}
	}

	onMount(() => {
		window.addEventListener('mouseup', handleMouseup);
		window.addEventListener('mousemove', (e) => {
			handleMousemove(e, 'size', 8, 320);
			handleMousemove(e, 'weight', 100, 900);
		});
		document.addEventListener('mousedown', handleClickOutside);

		return () => {
			window.removeEventListener('mouseup', handleMouseup);
			window.removeEventListener('mousemove', handleMousemove);
			document.removeEventListener('mousedown', handleClickOutside);
		};
	});
</script>

<div class="inputWrapper">
	<div class="inputContainer">
		<!-- Text Input -->
		<div class="txtPrv">
			<div class="floatingLabel">
				<input
					type="text"
					id="userInput"
					placeholder=" "
					bind:value={userText}
				/>
				<label for="userInput">What you type is what you get</label>
			</div>
		</div>

		<!-- Font Size Control -->
		<div class="txtSz">
			<div class="dropdown">
				<button
					class="dropbtn"
					on:click={() => (showSizeDropdown = !showSizeDropdown)}
				>
					{fontSize}px
				</button>
				{#if showSizeDropdown}
					<div class="dropdownContent" id="sizeDropdown">
						<div class="dropdownInner">
							{#each fontSizes as size}
								<a
									href="#"
									on:click|preventDefault={() => {
										fontSize = size;
										showSizeDropdown = false;
									}}
								>
									{size}px
								</a>
							{/each}
						</div>
					</div>
				{/if}
			</div>
			<div
				class="slider"
				id="sizeSlider"
				on:mousedown={() => handleMousedown('size')}
			>
				<div
					class="sliderTrack"
					style="width: {((fontSize - 8) / (320 - 8)) * 100}%"
				></div>
				<div
					class="sliderThumb"
					style="left: calc({((fontSize - 8) / (320 - 8)) * 100}% - 8px)"
				>
					<div>{fontSize}px</div>
				</div>
			</div>
		</div>

		<!-- Font Weight Control -->
		<div class="txtWgt">
			<div class="dropdown">
				<button
					class="dropbtn"
					on:click={() => (showWeightDropdown = !showWeightDropdown)}
				>
					{fontWeight}
				</button>
				{#if showWeightDropdown}
					<div class="dropdownContent" id="weightDropdown">
						<div class="dropdownInner">
							{#each fontWeights as weight}
								<a
									href="#"
									on:click|preventDefault={() => {
										fontWeight = weight;
										showWeightDropdown = false;
									}}
								>
									{weight}
								</a>
							{/each}
						</div>
					</div>
				{/if}
			</div>
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<div
				class="slider"
				id="weightSlider"
				on:mousedown={() => handleMousedown('weight')}
			>
				<div
					class="sliderTrack"
					style="width: {((fontWeight - 100) / (900 - 100)) * 100}%"
				></div>
				<div
					class="sliderThumb"
					style="left: calc({((fontWeight - 100) / (900 - 100)) * 100}% - 8px)"
				>
					<div>{fontWeight}</div>
				</div>
			</div>
		</div>
	</div>
</div>

<!-- Preview Area -->
<div class="displayContainer">
	<div
		class="fontPreview"
		style="font-size: {fontSize}px; font-weight: {fontWeight}"
	>
		{userText || 'Pack my box with five dozen liquor jugs'}
	</div>
</div>

<style lang="scss">
	@use 'sass:math';
	@import '../../styles/variables';

	.container {
		position: relative;
		display: flex;
		max-width: 100%;
		height: auto;
		min-height: 100vh;
		padding: ui(60);

		@include mobile {
			padding: ui(20);
		}
	}

	.wrapper {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: flex-start;
		position: relative;
		min-height: 100vh;
		width: 100%;
		// background: oklch(21.81% 0.02 248.75 / 80%);
		backdrop-filter: blur(ui(25));
		padding: ui(60) 0;
		border-top: ui(2) solid oklch(46.41% 0.055 248.2 / 35%);
		border-right: ui(2) solid oklch(46.41% 0.055 248.2 / 35%);
		border-bottom: ui(2) solid oklch(46.41% 0.055 248.2 / 10%);
		border-left: ui(2) solid oklch(46.41% 0.055 248.2 / 10%);
		border-radius: ui(24);
	}

	.alphaWrapper {
		position: relative;
		width: 100%;
		padding: 0 ui(60);
		margin-bottom: ui(128);

		@include mobile {
			padding: 0 ui(20);
			margin-bottom: ui(64);
		}
	}

	.inputWrapper {
		position: sticky;
		top: 0;
		width: 100%;
		padding: ui(12) ui(60);
		margin-bottom: ui(32);
		z-index: 1;
		transition: 0.3s ease-in-out;

		&.stickyActive {
			background: oklch(21.81% 0.02 248.75 / 95%);
		}

		@include mobile {
			padding: ui(12) ui(20);
		}
	}

	.inputContainer {
		position: relative;
		width: 100%;
		display: grid;
		grid-template-columns: 3fr 1fr 1fr;
		grid-template-areas: 'txt-prv txt-sz txt-wgt';
		gap: ui(32);
		color: white;
		z-index: 1;

		@include tablet {
			grid-template-columns: 1fr 1fr;
			grid-template-rows: auto auto;
			grid-template-areas:
				'txt-prv txt-prv'
				'txt-sz txt-wgt';
			gap: ui(20);
		}
	}

	.floatingLabel {
		position: relative;
		display: flex;
		align-items: center;

		label {
			position: absolute;
			top: 50%;
			transform: translateY(-50%);
			left: ui(32);
			font-size: ui(16);
			color: oklch(46.65% 0.055 246.35);
			pointer-events: none;
			transition: 0.3s ease-in-out;
		}

		input:placeholder-shown + label {
			font-size: ui(16);
			color: oklch(46.65% 0.055 246.35);
		}

		input:hover:placeholder-shown + label {
			color: oklch(63.69% 0.063 248.08);
		}

		input:not(:placeholder-shown) + label,
		input:focus + label {
			display: inline-flex;
			align-items: center;
			top: 0;
			height: ui(24);
			font-size: ui(14);
			color: oklch(63.69% 0.063 248.08);
			background: oklch(21.81% 0.02 248.75);
			padding: 0 ui(6);
		}
	}

	#userInput {
		width: 100%;
		background: transparent;
		border: ui(2) solid oklch(42.63% 0.051 250.1);
		border-radius: ui(32);
		font-size: ui(16);
		color: white;
		padding: ui(18) ui(32);
		transition: 0.3s ease-in-out;

		&:hover {
			border-color: oklch(63.69% 0.063 248.08);
		}

		&:focus {
			border-color: oklch(63.69% 0.063 248.08);
			box-shadow: inset 0 0 0 ui(2) oklch(63.69% 0.063 248.08);
			outline: none;
		}
	}

	.txtSz,
	.txtWgt {
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
		align-self: stretch;
		gap: ui(24);
		border: ui(2) solid oklch(42.63% 0.051 250.1);
		border-radius: ui(48);
		padding: 6% 12%;

		&::before {
			position: absolute;
			display: inline-flex;
			align-items: center;
			top: ui(-12);
			font-size: ui(14);
			height: ui(24);
			color: oklch(56.75% 0.071 249.83);
			background: oklch(21.81% 0.02 248.75);
			padding: 0 ui(6);
		}

		@include mobile {
			padding: 16%;
		}
	}

	.txtSz::before {
		content: 'Font Size';
	}

	.txtWgt::before {
		content: 'Font Weight';
	}

	.dropdown {
		position: relative;
		display: inline-block;

		@include mobile {
			display: none;
		}
	}

	.dropbtn {
		display: flex;
		align-items: center;
		justify-content: center;
		width: ui(80);
		background-color: transparent;
		color: oklch(56.75% 0.071 249.83);
		padding: ui(12) ui(16);
		border: ui(1) solid oklch(42.63% 0.051 250.1);
		border-radius: ui(4);
		cursor: pointer;
		transition: 0.3s ease-in-out;

		&:hover {
			color: oklch(70.07% 0.051 253.34);
			background-color: oklch(42.63% 0.051 250.1);
		}
	}

	.dropdownContent {
		display: none;
		position: absolute;
		background-color: oklch(35.05% 0.042 252.97 / 95%);
		width: ui(80);
		height: ui(128);
		border-radius: ui(4);
		box-shadow: 0 ui(8) ui(16) oklch(0% 0 0 / 20%);
		margin-top: ui(4);
		z-index: 1;
		overflow: hidden;

		a {
			font-size: 0.85em;
			color: oklch(59.44% 0.07 253.78);
			padding: ui(12) ui(16);
			text-decoration: none;
			display: block;
			transition: 0.2s ease-in-out;

			&:hover {
				background-color: oklch(23.11% 0.023 251.48 / 65%);
			}
		}
	}

	.slider {
		width: 50%;
		height: ui(4);
		background-color: oklch(31.46% 0.034 248.94);
		position: relative;
		border-radius: ui(10);

		@include mobile {
			width: 100%;
		}
	}

	.sliderTrack {
		position: absolute;
		height: ui(4);
		background-color: oklch(60.33% 0.069 248.36);
		border-radius: ui(10);
	}

	.sliderThumb {
		width: ui(16);
		height: ui(16);
		background-color: oklch(60.33% 0.069 248.36);
		border-radius: 50%;
		position: absolute;
		top: 50%;
		transform: translateY(-50%);
		cursor: pointer;

		div {
			position: absolute;
			color: oklch(56.75% 0.071 249.83);
			bottom: ui(-16);
			font-size: ui(11);
			left: 50%;
			transform: translateX(-50%);
		}
	}

	.displayContainer {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		min-height: ui(50);

		@include mobile {
			padding: ui(42) ui(20);
		}
	}

	.fontPreview {
		line-height: 1.1em;
		color: black;
		word-break: break-word;
		font-family: 'Mercator';
	}
</style>
