<script>
	import { onMount } from 'svelte';
	import { createEventDispatcher } from 'svelte';

	let allMovies = [];
	let selectedMovies = [];

	const dispatch = createEventDispatcher();

	function onChange() {
		dispatch('filterByMovies', {
			selectedMovies
		});
	}

	onMount(async () => {
		const res = await fetch(`https://swapi.dev/api/films/`);
		const data = await res.json();

		allMovies = data.results;
	});
</script>

<section class="filters">
	<div class="container">
		<div class="header">Filter by movies</div>
			<div class="content">
				<div class="options">
					{#each allMovies as movie (movie.release_date)}
						<div class="option" class:option--selected={selectedMovies.indexOf(movie.url) !== -1}>
							<label>
								<input type=checkbox bind:group={selectedMovies} value={movie.url} on:change="{onChange}">
								<div class="selection">X</div>
									{movie.title}
							</label>
						</div>
					{/each}
				</div>
			</div>
	</div>
</section>

<style>
	.container {
		position: relative;
		padding: 1.5rem 2rem 1rem;
    border: 1px solid #616161;
    border-radius: 0.2rem;
	}

	.header {
		position: absolute;
    top: -1rem;
    background: white;
    padding: .5rem 1rem;
	}

	.options {
		display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    gap: 1rem;
	}

	.option {
		position: relative;
		padding: .5rem 1rem;
		color: #616161;
    border-radius: 0.2rem;
		border-width: 2px;
    border-style: solid;
		border-color: #e0e0e0;
	}

	.option:not(.option--selected):hover {
		color: #eabe0f;
		border-color: #eabe0f;
	}

	.option--selected {
		border-color: #eabe0f;
    background-color: #eabe0f;
    color: white;
	}

	.selection {
		color: transparent;
		position: absolute;
  	top: 0;
  	left: 0;
	} 

	/* Hide default checkbox */
	input {
		position: absolute;
  	opacity: 0;
  	cursor: pointer;
  	height: 0;
  	width: 0;
	}
</style>
