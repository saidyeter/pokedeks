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

	import PokeItem from '../../lib/pokemon-item.svelte';

	function imgs(pokemonDetail) {
		let imgList = Object.getOwnPropertyNames(pokemonDetail.sprites)
			.filter(
				(x) => pokemonDetail.sprites[x] != null && typeof pokemonDetail.sprites[x] == 'string'
			)
			.reverse()
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

	function getImgSrc(id) {
		var imgSrc = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`;
		return imgSrc;
	}
	var speciesPromise = getPokemonSpecies(pokemonId);
</script>

{#await fetcher(`https://pokeapi.co/api/v2/pokemon/${pokemonId}`)}
	<p>loading</p>
{:then pokemonDetail}
	<div class="container">
		<div class="left">
			<div class="banner">
				<img
					src="https://pokeres.bastionbot.org/images/pokemon/{pokemonDetail.id}.png"
					alt={pokemonDetail.name}
				/>
			</div>
			<div class="forms">
				{#await speciesPromise}
					<p>loading</p>
				{:then data}
					{#await fetcher(data.evolution_chain.url)}
						<p>loading</p>
					{:then data}
						{#each parseEvolves(data) as poki}
							<PokeItem selected={poki.id == pokemonId} id={poki.id} imgSrc={getImgSrc(poki.id)} name={poki.name} />
						{/each}
					{/await}
				{/await}
			</div>
			<div class="stats">
				<h2>stats</h2>
				<div class="stats-container">
					{#each stats(pokemonDetail) as stat}
						<div class="stats-item global-card">
							<div class="stats-name">{stat.name}</div>

							<div class="stats-val">{stat.val}</div>
						</div>
					{/each}
				</div>
			</div>
		</div>
		<div class="main">
			<div class="header">
				<h1>{pokemonDetail.name}</h1>
				<div class="small-imgs">
					{#each imgs(pokemonDetail) as imgItem}
						<img src={imgItem.src} alt={imgItem.alt} />
					{/each}
				</div>
				<div class="info">
					<div class="info-item global-card">
						<div class="info-item-key">Poke Id</div>
						<div class="info-item-value">{pokemonDetail.id}</div>
					</div>
					<div class="info-item global-card">
						<div class="info-item-key">Height</div>
						<div class="info-item-value">{pokemonDetail.height}</div>
					</div>
					<div class="info-item global-card">
						<div class="info-item-key">Weight</div>
						<div class="info-item-value">{pokemonDetail.weight}</div>
					</div>
					<div class="info-item global-card">
						<div class="info-item-key">Base Experience</div>
						<div class="info-item-value">{pokemonDetail.base_experience}</div>
					</div>
				</div>
			</div>
			<div class="additional">
				{#await speciesPromise}
					<p>loading</p>
				{:then data}
					<div class="info">
						<div class="info-item global-card">
							<div class="info-item-key">Base Happiness</div>
							<div class="info-item-value">{data.base_happiness}</div>
						</div>
						<div class="info-item global-card">
							<div class="info-item-key">Capture Rate</div>
							<div class="info-item-value">{data.capture_rate}</div>
						</div>
						<div class="info-item global-card">
							<div class="info-item-key">Color</div>
							<div class="info-item-value">{data.color.name}</div>
						</div>
						<div class="info-item global-card">
							<div class="info-item-key">Growth Rate</div>
							<div class="info-item-value">{data.growth_rate.name}</div>
						</div>
						<div class="info-item global-card">
							<div class="info-item-key">Generation</div>
							<div class="info-item-value">{data.generation.name}</div>
						</div>
						<div class="info-item global-card">
							<div class="info-item-key">Is Baby</div>
							<div class="info-item-value">{data.is_baby}</div>
						</div>
						<div class="info-item global-card">
							<div class="info-item-key">Is Legendary</div>
							<div class="info-item-value">{data.is_legendary}</div>
						</div>
						<div class="info-item global-card">
							<div class="info-item-key">Is Mythical</div>
							<div class="info-item-value">{data.is_mythical}</div>
						</div>
						<div class="info-item global-card">
							<div class="info-item-key">Shape</div>
							<div class="info-item-value">{data.shape.name}</div>
						</div>
						<div class="info-item global-card">
							<div class="info-item-key">Egg Groups</div>
							<div class="info-item-value">
								{#each data.egg_groups as egg}
									{egg.name}
								{/each}
							</div>
						</div>
					</div>
				{/await}
			</div>
			<div class="types-abilities">
				<div class="types">
					<h3>types</h3>

					<div class="flag-container">
						{#each pokemonDetail.types as data}
							<a href={data.type.url}>
								<div class="flag global-card hovarable">
									{data.type.name}
								</div>
							</a>
						{/each}
					</div>
				</div>

				<div class="abilities">
					<h3>abilities</h3>

					<div class="flag-container">
						{#each pokemonDetail.abilities as data}
							<a href={data.ability.url}>
								<div class="flag global-card hovarable">
									{data.ability.name}
								</div>
							</a>
						{/each}
					</div>
				</div>
			</div>
		</div>
	</div>
	<div class="moves">
		<h3>moves</h3>
		<div class="flag-container">
			{#each pokemonDetail.moves as data}
				<a href={data.move.url}>
					<div class="flag global-card hovarable">
						{data.move.name}
					</div>
				</a>
			{/each}
		</div>
	</div>
{/await}

<!-- 
<div class="info">
	<div class="info-item global-card">
		<div class="info-item-key">Base Happiness</div>
		<div class="info-item-value">{data.base_happiness}</div>
	</div> -->
<style>
	.container,
	.forms {
		display: flex;
	}
	.left {
		margin-right: 1em;
	}
	.header {
		display: flex;
		flex-direction: column;
	}

	.banner img {
		width: 25rem;
	}

	.stats-container {
		display: flex;
		flex-wrap: wrap;
	}
	.stats-item {
		margin: 0.5em;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		width: 4em;
		height: 4em;
		border-radius: 50%;
	}
	.stats-name,
	.info-item-key {
		text-transform: uppercase;
		text-align: center;
		font-size: 0.65em;
	}
	.stats-val,
	.info-item-value {
		font-weight: 600;
	}
	.flag-container {
		display: flex;
		justify-content: center;
		flex-wrap: wrap;
	}
	.flag {
		display: block;
		margin: 0.4em;
		padding: 0.2em;
		border-radius: 0.5em;
	}
	.info {
		display: flex;
		flex-wrap: wrap;
	}
	.info-item {
		border-radius: 0.5em;
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 0.4em;
		padding: 0.2em;
	}
	.types-abilities {
		display: flex;
		flex-wrap: wrap;
	}
	.types {
		margin-right: 2em;
	}
</style>
