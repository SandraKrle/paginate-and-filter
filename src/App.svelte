<script>
	import Filters from './Filters.svelte';
	import PeopleList from './PeopleList.svelte';
	import Pagination from './Pagination.svelte';
	import { onMount } from 'svelte';

	// People
	let currentPeople = [];
	let totalPeople = null;

	// Filters
	let selectedFilms = [];

	//Pagination 
	let paginationPages = [];
	let prevResultsUrl = '';
	let nextResultsUrl = '';
	let nextIndex = null;

	$: nextIndex = nextResultsUrl ? nextResultsUrl.charAt(nextResultsUrl.length - 1) : null;
	$: filteredList = currentPeople.filter((person) => {
		let shouldFilter = true;
		
		selectedFilms.forEach((film) => {
			let isMatch = false;
			if (person.films.indexOf(film) !== -1) {
				isMatch = true;
			} else {
				isMatch = false;
			} 

			if(!isMatch) {
				shouldFilter = false;
				return;
			}
			return isMatch;
		})
		
		return shouldFilter;
	});

	function filterByMovies(event) {
		selectedFilms = event.detail.selectedMovies;
	}
	
	async function fetchPage(event) {
		const res = await fetch(`https://swapi.dev/api/people/?page=${event.detail.index}`);
		const data = await res.json();
		currentPeople = data.results;
	  prevResultsUrl = data.previous;
		nextResultsUrl = data.next;
	}

	onMount(async () => {
		const res = await fetch(`https://swapi.dev/api/people/`);
		const data = await res.json();

		currentPeople = data.results;
		totalPeople = data.count;
	  prevResultsUrl = data.previous;
		nextResultsUrl = data.next;
		paginationPages = Array.from(Array(Math.round(totalPeople / currentPeople.length) + 1), (x, i) => i + 1);
	});
</script>


<div class="container">
	<h1>Star Wars Characters</h1>
	<main>
		<Filters on:filterByMovies={filterByMovies}/>

		<PeopleList currentPeople={currentPeople} selectedFilms={selectedFilms} />

		<Pagination pages={paginationPages} prevUrl={prevResultsUrl} nextUrl={nextResultsUrl} on:fetchPage={fetchPage}/>
	</main>
</div>


<style>
	.container {
		display: grid;
    flex-direction: column;
    justify-content: center;
	}
</style>
