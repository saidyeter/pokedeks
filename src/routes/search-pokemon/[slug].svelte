<script context="module">
	export function load({ page }) {
		const { slug } = page.params;
		return {
			props: {
				searchKey: slug
			}
		};
	}
</script>

<script>	
	export let searchKey;
	let pokemonList=[]
	import { search } from '../pokemons/pokemon-source';
	import PokeItem from '../../lib/pokemon-item.svelte';	
	function getImgSrc(id) {
		var imgSrc = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`;
		return imgSrc;
	}
	pokemonList= search(searchKey)
</script>
<svelte:head>
	<title>Search</title>
</svelte:head>

<div class="search-area">
	<input type="text" bind:value={searchKey} placeholder="Search">
	<button on:click={()=>document.location='/search-pokemon/'+searchKey}>Search</button>
	<button on:click={()=>document.location='/search-pokemon'}>Clear</button>
</div>

 
<div class="container">
	{#each pokemonList as pokemon}
		<PokeItem id={pokemon.id} imgSrc={getImgSrc(pokemon.id)} name={pokemon.name} />
	{/each}
</div>  
<style>
	.search-area{
		display: flex;
		align-items: center;
		justify-content: center;
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
</style>
