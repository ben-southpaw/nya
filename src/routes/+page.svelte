<script>
	import CharacterViewer from '../lib/components/CharacterViewer.svelte';
	import TypeTester from '../lib/components/TypeTester.svelte';
</script>

<section>
	<CharacterViewer />
	<!-- <TypeTester /> -->
</section>

<style lang="scss">
	@use 'sass:math';
	@import '../styles/variables';
	:global(html) {
		@include responsive-scale(740, 1440, 16/9);
		@media #{$breakpoint-small} {
			@include responsive-scale(
				$screen-min: 0,
				$screen-max: 800,
				$aspect-ratio: math.div(390, 844)
			);
		}

		&::-webkit-scrollbar {
			display: none;
		}
	}

	:global(body) {
		margin: 0;
		padding: 0;
	}

	section {
		width: 100vw;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
</style>
