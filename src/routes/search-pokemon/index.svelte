<script>
    let searchKey=""
</script>

<svelte:head>
	<title>Search</title>
</svelte:head>

<div class="header">
	<div></div>
	<div class="search">
		<input type="text" bind:value={searchKey} placeholder="Search">
		<button on:click={document.location='/search-pokemon/'+searchKey}>Search</button>
	</div>
	<div></div>
</div>

<style>
	.header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding-left: 1em;
		padding-right: 1em;
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
