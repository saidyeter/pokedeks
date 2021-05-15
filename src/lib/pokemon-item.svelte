<script>
	export let id;
	export let name;
	export let imgSrc;
</script>

<a href="/pokemons/{id}">
	<div class="container">
		<img src={imgSrc} loading="lazy" alt={name} />
		<span>{name}</span>
	</div>
</a>

<style>
	a {
		text-decoration: none;
		color: black;
	}
	.container {
		background-color: var(--primary-color);
		opacity: 0.8;
		color: #fff;
		width: 9em;
		height: 9em;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin: 1em;
		box-shadow: 0.5em 0.5em 0.8em rgba(0, 0, 0, 0.8);
		transition: .5s;
        border-radius: 1em;
	}
	img {
		transition: .5s;
		width: 5.6em;
		height: 5.6em;
	}
	.container:hover {
		box-shadow: 0.5em 0.5em 1em rgba(0, 0, 0, 0.6);
		opacity: 1;
		margin: 0.5em;
		width: 10em;
		height: 10em;
	}

	.container:hover img {
		width: 6em;
		height: 6em;
	}
</style>
