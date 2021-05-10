<script>
	const p = parseInt
	let limit = 25;
	let offset = 0;
	let count;
	async function fetchPokemons() {
		var response = await fetch(`https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=${limit}`);
		var data = await response.json();
		offset = p(limit) + p(offset)
		count = data.count
		return data.results.map((x) => {
			const obj = {
				name: x.name,
				url: x.url,
				id: getId(x.url),
				imgSrc: getImg(x.url)
			};
			return obj;
		});
	}

	let promise = fetchPokemons();

	function handleClick() {
		promise = fetchPokemons();
	}

	function getId(url) {
		var parts = url.split('/'); //https://pokeapi.co/api/v2/pokemon/1/
		var id = parts[parts.length - 2];
		return id;
	}

	function getImg(url) {
		var parts = url.split('/'); //https://pokeapi.co/api/v2/pokemon/1/
		var id = parts[parts.length - 2];
		var imgSrc = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`;
		return imgSrc;
	}
</script>

<header>
	<label
		>Limit :
		<select bind:value={limit} on:change={handleClick}> 
			<option selected>25</option>
			<option>50</option>
			<option>100</option>
			<option>200</option>
		</select>
	</label>
	<button on:click={handleClick}> next</button>
	<span>Total Count = {count}</span>
</header>
<div class="container">
	<slot />
	{#await promise}
		<p>loading</p>
	{:then value}
		{#each value as pokemon}
			<div class="pokidiv">
				<a rel="prefetch" href="pokemons/{pokemon.id}">
					<img src={pokemon.imgSrc} loading="lazy" alt={pokemon.name} />
					<br />
					{pokemon.name}
				</a>
			</div>
		{/each}
	{/await}
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
</style>
