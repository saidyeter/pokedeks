<script>
	let limit = 50;
	let offset = 0;
	let letter = 'a';
	let count = 0;

	let pokemonList = [];
	import { get, search } from './pokemon-source';

	function fetchPokemons() {
		pokemonList = get(offset, limit, letter);
		count = pokemonList.length;
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

<header>
	<p>Listing {count} pokemons</p>
	<button on:click={getPrevious} disabled={offset <= 0}> previous</button>
	<label
		>Limit :
		<select bind:value={limit} on:change={fetchPokemons}>
			<option selected>50</option>
			<option>100</option>
			<option>200</option>
		</select>
	</label>
	<button on:click={getNext} disabled={offset >= count}> next</button>
	<br />
	<br />
	<ul>
		<li on:click={() => getCatalog('a')}>a</li>
		<li on:click={() => getCatalog('b')}>b</li>
		<li on:click={() => getCatalog('c')}>c</li>
		<li on:click={() => getCatalog('d')}>d</li>
		<li on:click={() => getCatalog('e')}>e</li>
		<li on:click={() => getCatalog('f')}>f</li>
		<li on:click={() => getCatalog('g')}>g</li>
		<li on:click={() => getCatalog('h')}>h</li>
		<li on:click={() => getCatalog('i')}>i</li>
		<li on:click={() => getCatalog('j')}>j</li>
		<li on:click={() => getCatalog('k')}>k</li>
		<li on:click={() => getCatalog('l')}>l</li>
		<li on:click={() => getCatalog('m')}>m</li>
		<li on:click={() => getCatalog('n')}>n</li>
		<li on:click={() => getCatalog('o')}>o</li>
		<li on:click={() => getCatalog('p')}>p</li>
		<li on:click={() => getCatalog('q')}>q</li>
		<li on:click={() => getCatalog('r')}>r</li>
		<li on:click={() => getCatalog('s')}>s</li>
		<li on:click={() => getCatalog('t')}>t</li>
		<li on:click={() => getCatalog('u')}>u</li>
		<li on:click={() => getCatalog('w')}>w</li>
		<li on:click={() => getCatalog('x')}>x</li>
		<li on:click={() => getCatalog('y')}>y</li>
		<li on:click={() => getCatalog('z')}>z</li>
	</ul>
</header>
<div class="container">
	<slot />
	{#each pokemonList as pokemon}
		<div class="pokidiv">
			<a rel="prefetch" href="pokemons/{pokemon.id}">
				<img src={getImgSrc(pokemon.id)} loading="lazy" alt={pokemon.name} />
				<br />
				{pokemon.name}
			</a>
		</div>
	{/each}
</div>

<style>
	.container {
		display: flex;
		flex-wrap: wrap;
	}
	.pokidiv {
		width: 120px;
		height: 120px;
	}
	ul {
		margin-block-start: 0em;
		margin-block-end: 0em;
		padding-inline-start: 0em;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		width:auto;
		
	}
	ul li {
		list-style: none;
		cursor: pointer;
		border: 1px solid black;
	}
</style>
