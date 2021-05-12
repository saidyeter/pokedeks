<script context="module">
	export function load({ page }) {
		const { slug } = page.params;
		return {
			props: {
				pokemonId: slug
			}
		};
	}
</script>

<script>
	export let pokemonId;

	// let pokemonDetail;
	// let imgList;
	// let statList;
	// async function prepare(id) {
	// 	console.log("id",id)
	// 	pokemonDetail = await fetcher(`https://pokeapi.co/api/v2/pokemon/${id}`);
	// 	imgs();
	// 	stats();
	// }

	function imgs(pokemonDetail) {
		let imgList = Object.getOwnPropertyNames(pokemonDetail.sprites)
			.filter(
				(x) => pokemonDetail.sprites[x] != null && typeof pokemonDetail.sprites[x] == 'string'
			)
			.map((x) => {
				return {
					alt: x,
					src: pokemonDetail.sprites[x]
				};
			});
		return imgList;
	}

	function getId(url) {
		var parts = url.split('/'); //https://pokeapi.co/api/v2/pokemon/1/
		var id = parts[parts.length - 2];
		//console.log("url",url,"id",id)
		return id;
	}
	function stats(pokemonDetail) {
		let statList = pokemonDetail.stats.map((x) => {
			return {
				val: x.base_stat,
				name: x.stat.name
			};
		});
		return statList;
	}

	async function getPokemonSpecies(id) {
		var url = `https://pokeapi.co/api/v2/pokemon-species/${id}`;
		return await fetcher(url);
	}

	async function fetcher(url) {
		var response = await fetch(url);
		var data = await response.json();
		return data;
	}

	function getImg(url) {
		var parts = url.split('/'); //https://pokeapi.co/api/v2/pokemon/1/
		var id = parts[parts.length - 2];
		var imgSrc = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`;
		return imgSrc;
	}

	function parseEvolves(data) {
		var res = [];
		var d = data.chain;
		res.push({
			id: getId(d.species.url),
			name: d.species.name,
			imgSrc: getImg(d.species.url)
		});

		while (d.evolves_to.length != 0) {
			d = d.evolves_to[0];
			res.push({
				id: getId(d.species.url),
				name: d.species.name,
				imgSrc: getImg(d.species.url)
			});
		}
		return res;
	}


</script>

{#await fetcher(`https://pokeapi.co/api/v2/pokemon/${pokemonId}`)}
	<p>loading</p>
{:then pokemonDetail} 
	
<div class="container">
	<div class="col-1">
		<img
			src="https://pokeres.bastionbot.org/images/pokemon/{pokemonDetail.id}.png"
			alt={pokemonDetail.name}
		/>
		<h1>{pokemonDetail.name}</h1>

		<ul>
			<li>Name:{pokemonDetail.name}</li>
			<li>Poke Id :{pokemonDetail.id}</li>
			<li>Height:{pokemonDetail.height}</li>
			<li>Weight:{pokemonDetail.weight}</li>
			<li>Base Experience:{pokemonDetail.base_experience}</li>
		</ul>
	</div>
	<div class="col-2">
		<h2>stats</h2>
		<ul>
			{#each stats(pokemonDetail) as stat}
				<li>{stat.name} : {stat.val}</li>
			{/each}
		</ul>
		<h2>abilities</h2>
		<ul>
			{#each pokemonDetail.abilities as data}
				<li><a href={data.ability.url}>{data.ability.name}</a></li>
			{/each}
		</ul>
		<h2>moves</h2>
		<ul>
			<!-- buraya buton ekleyip dahasını getirmek gerekli -->
			{#each pokemonDetail.moves.slice(0, 10) as data}
				<li><a href={data.move.url}>{data.move.name}</a></li>
			{/each}
		</ul>
	</div>
	<div class="col-3">
		{#each imgs(pokemonDetail) as imgItem}
			<img src={imgItem.src} alt={imgItem.alt} />
		{/each}
	</div>
</div>
<div class="species">
	{#await getPokemonSpecies(pokemonId)}
		<p>loading</p>
	{:then data}
		<div class="info">
			<p>Base Happiness :{data.base_happiness}</p>
			<p>Capture Rate:{data.capture_rate}</p>
			<p>Color:{data.color.name}</p>
			<p>Growth Rate:{data.growth_rate.name}</p>
			<!-- <p>Habitat:{data.habitat.name}</p> -->
			<p>Generation:{data.generation.name}</p>
			<p>Is Baby:{data.is_baby}</p>
			<p>Is Legendary:{data.is_legendary}</p>
			<p>Is Mythical:{data.is_mythical}</p>
			<p>Shape:{data.shape.name}</p>
			<ul>
				{#each data.egg_groups as egg}
					<li>{egg.name}</li>
				{/each}
			</ul>
		</div>
		<div class="forms">
			{#await fetcher(data.evolution_chain.url)}
				<p>loading</p>
			{:then data}
				{#each parseEvolves(data) as poki}
					<div class="pokidiv">
						<a href="/pokemons/{poki.id}">
							<img src={poki.imgSrc} loading="lazy" alt={poki.name} />
							<br />
							{poki.name}
						</a>
					</div>
				{/each}
			{/await}
		</div>
	{/await}
</div>

{/await}
<style>
	.container,
	.species {
		width: auto;
		display: flex;
		flex: 1;
	}
	.container,
	.species div {
		border: 1px solid black;
		width: auto;
		min-width: 30%;
	}
	.col-1 img {
		width: 15rem;
	}

	.pokidiv {
		width: 120px;
		height: 120px;
	}
	.forms {
		display: flex;
		flex-wrap: wrap;
	}
</style>
