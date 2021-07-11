<script>
	export let name;
	import Typing from './components/Typing.svelte'
</script>

<main>
	<h1>Hello {name}!</h1>
	<!-- <p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p> -->
	<Typing />

</main>

<style>
	:global(body) {
		background-color: #212121;
		color: white
	}

	main {
		text-align: center;
		padding: 1em;
		max-width: 900px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
</style>