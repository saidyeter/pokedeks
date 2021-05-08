<script context="module">
	export const prerender = true;
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>
<slot />

<section>
	<div>
		<h1>POKEDEKS</h1>
		<p>
			This page is uses <a href="https://pokeapi.co/docs/v2 "> pokeapi</a>
			<br />
			For bigger images <a href="https://pokeres.bastionbot.org">pokeres.bastionbot</a>
			<br />
			This site written for learning purpose
		</p>
	</div>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}

	h1 {
		width: 100%;
	}

</style>
