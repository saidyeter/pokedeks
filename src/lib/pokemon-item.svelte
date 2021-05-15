<script>
	export let id;
	export let name;
	export let imgSrc;
	export let selected;
</script>

<a href="/pokemons/{id}">
	<div class="container global-card hovarable {selected == true ? 'selected' : ''}">
		<img src={imgSrc} loading="lazy" alt={name} />
		<span>{name}</span>
	</div>
</a>

<style>
	.container {
		width: 9em;
		height: 9em;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin: 1em;
		border-radius: 1em;
	}
	img {
		transition: 0.5s;
		width: 5.6em;
		height: 5.6em;
	}
	.container:hover {
		margin: 0.5em;
		width: 10em;
		height: 10em;
	}

	.container:hover img {
		width: 6em;
		height: 6em;
	}
	.selected {
		border: 0.1em solid var(--selected);
	}
</style>
