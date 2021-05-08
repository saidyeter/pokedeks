<script context="module">
	export async function load({ page }) {
		const { slug } = page.params;
		var response = await fetch(`https://pokeapi.co/api/v2/pokemon/${slug}`);
		var data = await response.json();
		return {
			props: {
				pokemonDetail: data
			}
		};
	}
</script>

<script>
	export let pokemonDetail;
	let imgList;
	let statList;
	function imgs() {
		imgList = Object.getOwnPropertyNames(pokemonDetail.sprites)
			.filter(
				(x) => pokemonDetail.sprites[x] != null && typeof pokemonDetail.sprites[x] == 'string'
			)
			.map((x) => {
				return {
					alt: x,
					src: pokemonDetail.sprites[x]
				};
			});
	}
	
	function getId(url) {
		var parts = url.split('/'); //https://pokeapi.co/api/v2/pokemon/1/
		var id = parts[parts.length - 2];
		return id;
	}
	function stats() {
		statList = pokemonDetail.stats
			.map((x) => {
				return {
					val: x.base_stat,
					name: x.stat.name
				};
			});
	}

	imgs();
	stats()
	/*	
	-büyük resim 
	-isim
	-abilities
	-base_experience
	---forms
	-moves-hareketleri
	-poke id 
	species-özellikler
	-sprites-resimleri 
	-stats-güçleri
	-weight-
	-height
	*/
</script>

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
			<li>Base_experience:{pokemonDetail.base_experience}</li>
		</ul>
	</div>
	<div class="col-2">
		<!-- <h2>forms</h2>
		<ul>
			{#each pokemonDetail.forms as data}
				<li><a href="pokemons/{getId(data.url)}">{data.name}</a></li>
			{/each}
		</ul> -->
		<h2>stats</h2>
		<ul>
			{#each statList as stat}
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
		{#each imgList as imgItem}
			<img src={imgItem.src} alt={imgItem.alt} />
		{/each}
	</div>
</div>

<style>
	.container {
		width: auto;
		display: flex;
		flex: 1;
	}
	.container div {
		border: 1px solid black;
		width: auto;
		min-width: 30%;
	}
	.col-1 img {
		width: 15rem;
	}

</style>
