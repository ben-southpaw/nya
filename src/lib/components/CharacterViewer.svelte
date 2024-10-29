<script>
	// Props
	// export let isUppercase = true;

	// Create alphabet grid
	const alphabet =
		` !"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[/]_abcdefghijklmnopqrstuvwxyz{|}~`.split(
			''
		);

	let current = 'A';
	$: current;

	function handleHover() {
		current = this.innerHTML;
	}
</script>

<section>
	<div class="letter-container">
		<div class="letter-info">
			<div class="letter">
				<div class="cap-height"></div>
				<p>{current}</p>
			</div>
		</div>
	</div>

	<div class="alphaWrapper">
		<div class="gridContainer">
			{#each alphabet as letter}
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<div class="gridItem" on:mouseenter={() => (current = letter)}>
					<p>
						{letter}
					</p>
				</div>
			{/each}
		</div>
	</div>
</section>

<style lang="scss">
	@use 'sass:math';
	@import '../../styles/variables';

	:global(html) {
		@include responsive-scale(
			$screen-min: 740,
			$screen-max: 1440,
			$aspect-ratio: math.div(1440, 740)
		);
		@media #{$breakpoint-small} {
			@include responsive-scale(
				$screen-min: 0,
				$screen-max: 800,
				$aspect-ratio: math.div(390, 844)
			);
		}
	}

	section {
		display: grid;
		grid-template-columns: minmax(0, min(40%, 500px)) auto;
		grid-gap: 30px;
		position: relative;
		max-height: 100vh;

		--units-per-em: 1000;
		--ascender: 1015;
		--descender: -185;
		--cap-height: 700;
		--x-height: 471;
		--line-gap: 315;
		--monitor-width: 480;
	}
	.letter-container {
		// width: 50vw;
		// height: 10vh;
		// display: flex;
		// align-items: center;
		--size: calc(var(--monitor-width) * 1 / 2);
		--half-gap: calc(var(--line-gap) / 2);
		--scale: calc(var(--size) / var(--units-per-em));
		font-size: 1rem;
		line-height: normal;
	}

	.letter-info {
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;
		min-width: 40vw;
		margin-bottom: 32px;
		height: calc(
			1px * (var(--ascender) - var(--descender) + var(--line-gap)) *
				var(--scale)
		);

		.cap-height {
			position: absolute;
			border-top: ui(1) solid black;
			top: ui(80);
			width: 100%;
		}

		.letter {
			font-size: calc(1px * var(--size));
			line-height: normal;
			text-align: center;
			white-space: pre;
			position: relative;
			z-index: 1;
			p {
				font-family: 'Mercator';
				font-size: ui(99);
				font-weight: 800;
				font-feature-settings:
					'calt' 0,
					'liga' 0;
			}
		}
	}

	.alphaWrapper {
		position: relative;
		width: calc(50% - ui(60));
		padding: ui(60);
		margin-bottom: ui(128);

		@include mobile {
			padding: 0 ui(20);
			margin-bottom: ui(64);
		}
	}

	.gridContainer {
		display: grid;
		grid-template-columns: repeat(11, ui(50));
		grid-auto-rows: ui(50);
		width: 100%;
		height: 100%;

		@include tablet {
			grid-template-columns: repeat(6, 1fr);
		}

		@include mobile {
			grid-template-columns: repeat(4, 1fr);
		}
	}
	.gridItem {
		position: relative;
		color: oklch(63.69% 0.063 248.08);
		display: flex;
		justify-content: center;
		align-items: center;
		font-weight: 300;
		transition: 0.1s ease-out;
		will-change: background-color;
		--border: 1px;
		border: var(--border) solid black;
		margin-left: calc(var(--border) * -1);
		margin-bottom: calc(var(--border) * -1);

		&:hover {
			// scale: 1.1;
			// border-radius: ui(5);
			// font-weight: 900;
			box-shadow: 0 ui(5) ui(20) oklch(0% 0 0 / 65%);
			background-color: black;
			color: #fff;
		}

		p {
			font-size: ui(23);
			line-height: calc(ui(23) * 0.2);
			font-family: 'Mercator';
		}
	}
</style>
