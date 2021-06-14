<script>
	import { fade } from 'svelte/transition';
  import { createEventDispatcher } from 'svelte';

	export let pages;
	export let prevUrl;
	export let nextUrl;

  const dispatch = createEventDispatcher();

	$: nextIndex = nextUrl ? nextUrl.charAt(nextUrl.length - 1) : null;
  $: prevIndex = prevUrl ? prevUrl.charAt(prevUrl.length - 1) : null;

  function handleClick(index) {
    	dispatch('fetchPage', {
			index
		});
  }
</script>


<div class="container">
  <section class="pagination">
    <div
      class:pagination__navigation--disabled="{!prevUrl}"
      class="pagination__navigation"
      role="button"
      disabled={!prevUrl}
      on:click={handleClick(prevIndex)}>
        «
    </div>
    {#each pages as index (index)}
      <div class="pagination__page" class:active="{index === nextIndex - 1}">
        {#if index === nextIndex - 1}
          <div
            class="pagination__background"
            transition:fade="{{ duration: 200 }}" 
          />
        {/if}
        <div
          class="pagination__index"
          role="button"
          data-text={index}
          on:click={handleClick(index)}>
            {index}
        </div>
      </div>
    {/each}
    
    <div
      class:pagination__navigation--disabled="{!nextUrl}"
      class="pagination__navigation"
      role="button"
      disabled={!nextUrl}
      on:click={handleClick(nextIndex)}>
        »
    </div>
  </section>
</div>

<style>
	.container {
		display: grid;
    flex-direction: column;
    justify-content: center;
	}

	.pagination {
		display: flex;
    flex-direction: row;
		justify-content: center;
		gap: 1.5rem;
		margin: 3rem 0;

		--bold: 800;
	}

	.pagination__page {
		position: relative;
		cursor: pointer;
	}

	.pagination__index {
		display: inline-flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-between;
		color: black;
		text-decoration: none;
	}

	.pagination__index:hover {
		font-weight: var(--bold);
		transition: all cubic-bezier(0, 1.22, 0.61, 0.85) .2s;
	}

	/* To offset the layout shift when the text is bold on hover */
	.pagination__index::after {
		content: attr(data-text);
  	height: 0;
  	visibility: hidden;
  	overflow: hidden;
  	user-select: none;
  	pointer-events: none;
  	font-weight: var(--bold);
	}

  @media speech {
		.pagination__index::after {
			display: none;
		}
	}

	.active .pagination__index {
		color: white;
		font-weight: var(--bold);
	}

	.active .pagination__background {
		position: absolute;
    padding: 1rem;
    border-radius: 50%;
    background-color: #eabe0f;
    top: -0.43rem;
    right: -0.73rem;
    z-index: -1;
	}

	.pagination__navigation {
		cursor: pointer;
		font-weight: var(--bold);
	}

	.pagination__navigation--disabled {
		color: #d7d7d7;
		cursor: auto;
	}

</style>
