<script>
	// type stuff here
</script>

<main>
	<svg width='1900' height='250' xmlns="http://www.w3.org/2000/svg">
		<rect width="100%" height="100%" style="fill:lightblue;" />
		<text x= 450 y = 200 fill = 'navy' font-family='Latos' font-size= 75>Medicare Star Insights</text>
	</svg>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
</main>

<style>

	main {
		text-align: center;
		max-width: 240px;
		margin: 0 auto;
		padding: 0;
	}

	h1 {
		color: navy;
		text-transform: uppercase;
		font-family: 'Lato', sans-serif;
		font-size: 4em;
		font-weight: 100;
		position: absolute;
		text-align: center;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>