<script>
	const alphabet =
		` !"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[/]_abcdefghijklmnopqrstuvwxyz{|}~`.split(
			''
		);

	const latinOne =
		`ÀÁÂÃÄÅÆÇÈÉÊËÌÍÎÏÐÑÒÓÔÕÖØÙÚÛÜÝÞßàáâãäåæçèéêëìíîïðñòóôõöøùúûüýþÿ¡¢£¤¥¦§¨©ª«¬®¯°±²³´ µ ¶ · ¸ ¹ º » ¼ ½ ¾ ¿ × ÷`.split(
			''
		);

	let current = 'A';
	$: current;
	let hexCode = '';

	function handleHover(event) {
		current = event.target.querySelector('p').innerHTML;
		const codePoint = current.codePointAt(0);
		hexCode = `U+${codePoint.toString(16).toUpperCase()}`;
	}
</script>

<section>
	<div class="letter-container">
		<div class="letter-info">
			<div class="line ascender">
				<span>ascender</span>
			</div>
			<div class="line cap-height">
				<span>cap-height</span>
			</div>
			<div class="line xheight">
				<span>x-height</span>
			</div>
			<div class="line baseline">
				<span>baseline</span>
			</div>
			<div class="line descender">
				<span>descender</span>
			</div>
			<p class="letter">{current}</p>
			<div class="font-info">
				<p><span>Font name___</span> Mercator Nieuw Regular</p>
				<p><span>Glyph name___</span> name</p>
				<p>
					<span>Unicode hex___</span>
					{hexCode}
				</p>
				<p>
					<span>Unicode name___</span>
					here
				</p>
				<p><span>Unicode decimal___</span> {current.charCodeAt(0)}</p>
			</div>
		</div>
	</div>

	<div class="alphaWrapper">
		<div class="gridContainer">
			{#each alphabet as letter}
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<div class="gridItem" on:mouseenter={handleHover}>
					<p>
						{letter}
					</p>
				</div>
			{/each}
		</div>

		<!-- <div class="gridContainer">
			{#each latinOne as letter}
				<div class="gridItem" on:mouseenter={() => (current = letter)}>
					<p>
						{letter}
					</p>
				</div>
			{/each}
		</div> -->
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

	p {
		font-family: 'Mercator';
	}

	section {
		display: flex;
		position: relative;
		justify-content: space-around;
		max-height: 100vh;
		height: 100vh;
		// width: 100vw;

		--units-per-em: 1000;
		--ascender: 1015;
		--descender: -185;
		--cap-height: 700;
		--x-height: 471;
		--line-gap: 315;
		--monitor-width: 480;
	}
	.letter-container {
		--size: calc(var(--monitor-width) * 1 / 2);
		--half-gap: calc(var(--line-gap) / 2);
		--scale: calc(var(--size) / var(--units-per-em));
		position: sticky;
		margin-right: auto;
	}

	.letter-info {
		position: sticky;
		min-width: 42vw;
		// width: 50vw;
		margin-bottom: 32px;
		height: calc(
			1px * (var(--ascender) - var(--descender) + var(--line-gap)) *
				var(--scale)
		);
		top: 50%;
		transform: translateY(-50%);
		// background-color: aquamarine;

		.letter {
			font-size: calc(1px * var(--size));
			text-align: center;
			white-space: pre;
			z-index: 2;
			line-height: normal;
			font-family: 'Mercator';
			font-weight: 500;
			font-feature-settings:
				'calt' 0,
				'liga' 0;
		}

		.line {
			width: 100%;
			border-top: solid 1px #2b2b2b;
			position: absolute;
			opacity: 0.5;
			span {
				font-family: 'Mercator';
				position: absolute;
				bottom: -2.5vh;
				font-weight: 700;
				font-size: ui(4);
			}
		}

		.cap-height {
			top: calc(
				1px * (var(--ascender) + var(--half-gap) - var(--cap-height)) *
					var(--scale)
			);
		}
		.ascender {
			top: calc(1px * var(--half-gap) * var(--scale));
		}
		.xheight {
			top: calc(
				1px * (var(--ascender) - var(--x-height) + var(--half-gap)) *
					var(--scale)
			);
		}
		.baseline {
			top: calc(
				1px * (var(--ascender) + var(--half-gap)) * var(--scale) - 0.5px
			);
		}
		.descender {
			top: calc(
				1px * (var(--ascender) - var(--descender) + var(--half-gap)) *
					var(--scale)
			);
		}

		.font-info {
			position: absolute;
			bottom: -100%;
			height: 100%;
			width: 100%;
			p {
				margin: 0;
				font-style: italic;
				span {
					font-style: normal;
				}
			}
		}
	}

	.alphaWrapper {
		position: relative;
		padding: ui(60);
		margin-right: ui(-60);
		display: flex;
		align-items: center;
		justify-content: center;

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
		// height: 100%;

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
