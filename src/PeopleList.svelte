<script>
	export let currentPeople;
	export let selectedFilms;

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
</script>

<section class="people">
  {#each filteredList as person (person.url)}
    <div class="person__card">
      <h4>
        {person.name}
      </h4>
      {#if person.birth_year !== 'unknown'}
        <p>Born {person.birth_year}</p>
        {:else}
        <p>Unknown year of birth</p>
      {/if}
    </div>
  
  {:else}
    <p>loading...</p>
  {/each}
</section>

<style>
	.people {
		width: 100%;
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		grid-gap: 1.5rem;
		padding-top: 2rem;
	}

	.person__card {
		padding: 0 0.5rem;
		box-shadow: rgba(0, 0, 0, 0.15) 2.4px 2.4px 3.2px;	
}
</style>
