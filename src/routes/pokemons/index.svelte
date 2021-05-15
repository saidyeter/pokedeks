<script>

	import PokeItem from '../../lib/pokemon-item.svelte';
	let limit = 15;
	let offset = 0;
	let letter = 'a';
	let count = 0;
	let totalCount = 0;

	let pokemonList = [];
	import { get, search } from './pokemon-source';

	function fetchPokemons() {
		limit = parseInt(limit);
		offset = parseInt(offset);
		totalCount = parseInt(totalCount);
		count = parseInt(count);
		//console.log(offset, limit, letter);
		var data = get(offset, limit, letter);
		pokemonList = data.list;
		totalCount = parseInt(data.count);
		count = parseInt(pokemonList.length);
	}
	function getNext() {
		offset = parseInt(offset) + parseInt(limit);
		fetchPokemons();
		if (offset > count) {
		}
	}
	function getPrevious() {
		offset = parseInt(offset) - parseInt(limit);
		fetchPokemons();
	}

	function getImgSrc(id) {
		var imgSrc = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`;
		return imgSrc;
	}
	fetchPokemons();

	function getCatalog(letterValue) {
		offset = 0;
		letter = letterValue;
		fetchPokemons();
	}
</script>

<div class="header">
	<div class="actions">
		<button on:click={getPrevious} disabled={offset <= 0}> previous</button>
		<label
			>Limit :
			<select bind:value={limit}>
				<option selected>15</option>
				<option>20</option>
				<option>50</option>
			</select>
			<button on:click={fetchPokemons}> Apply</button>
		</label>
		<button on:click={getNext} disabled={offset + limit >= totalCount}> next</button>
	</div>
	<div class="info">
		<span>Listing from {offset + 1} to {offset + count} pokemons in {totalCount}</span>
	</div>
</div>
<div class="ul">
	<div class="li global-card" on:click={() => getCatalog('')}>All</div>
	{#each 'abcdefghijklmnopqrstuvwxyz'.split('') as letter}
		<div class="li global-card" on:click={() => getCatalog(letter)}>{letter}</div>
	{/each}
</div>
<br />
<br />
<div class="container">
	<slot />
	{#each pokemonList as pokemon}
		<PokeItem id={pokemon.id} imgSrc={getImgSrc(pokemon.id)} name={pokemon.name} />
	{/each}
</div>

<style>
	.header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding-left: 1em;
		padding-right: 1em;
	}
	.container {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		align-items: center;
		min-width: 10em;
	}
	.ul {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-wrap: wrap;
	}
	.li {
		opacity: 0.8;
		font-size: 1.2em;
		cursor: pointer;
		width: 1.6em;
		height: 1.6em;
		margin: 0.4em;
		display: flex;
		justify-content: center;
		align-items: center;
		box-shadow: 0.2em 0.2em 0.4em rgba(0, 0, 0, 0.8);
		transition: 0.2s;
		border-radius: .4em;
	}
	.li:hover {
		box-shadow: 0.2em 0.2em 0.6em rgba(0, 0, 0, 0.6);
		opacity: 1;
		margin: 0.2em;
		width: 2em;
		height: 2em;
	}
	button {
		background-color: var(--primary-color);
		opacity: 0.8;
		border: none;
		color: #fff;
		border-radius: 0.3em;
		transition: 0.2s;
		height: 1.6em;
		margin: 0.1em;

		box-shadow: 0.2em 0.2em 0.4em rgba(0, 0, 0, 0.8);
	}
	button:hover {
		opacity: 1;
		height: 1.8em;
		margin-bottom: 0em;
		margin-top: 0em;
		box-shadow: 0.2em 0.2em 0.5em rgba(0, 0, 0, 0.6);
	}
	button[disabled] {
		opacity: 0.2;
	}
</style>
