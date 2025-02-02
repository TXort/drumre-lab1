<script lang="ts">
	import { enhance } from '$app/forms';
	import ShowThumbnail from '$components/ShowThumbnail.svelte';
	import type { PageServerData } from './$types';

	let { data }: { data: PageServerData } = $props();
	let show = $derived(data.show!);
</script>

<svelte:head>
	<title>{show.title}</title>
</svelte:head>

<h1>{show.title}</h1>

<form method="POST" action="/shows?/removeFavourite" use:enhance id="remove-favourite"></form>
<form method="POST" action="/shows?/addFavourite" use:enhance id="add-favourite"></form>

<article class="show-details">
	{#if show?.imageLinks?.thumbnail}
		<img class="thumbnail" src={show.imageLinks.thumbnail} alt={show.title} />
	{:else if show.trailerLink}
		<iframe
			width="560"
			height="315"
			src={show.trailerLink}
			title="YouTube video player"
			frameborder="0"
			allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
			allowfullscreen
		></iframe>
	{/if}
	<div>
		<p>{show.overview}</p>
	</div>
</article>

{#if show?.imageLinks?.thumbnail && show.trailerLink}
	<div class="trailer-card">
		<iframe
			width="560"
			height="315"
			src={show.trailerLink}
			title="YouTube video player"
			frameborder="0"
			allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
			allowfullscreen
		></iframe>
	</div>
{/if}




<div class="data-grid">
	<span class="label">Release year:</span>
	<span>{show.year}</span>
	<span class="label">Genres:</span>
	<span>{show.genres}</span>
	<span class="label">Rating:</span>
	<span>{ show.rating.toFixed(2) }</span>
	<span class="label">Country:</span>
	<span>{show.country}</span>
	<span class="label">Language:</span>
	<span>{show.language}</span>
	<span class="label">Network:</span>
	<span>{show.network}</span>
</div>


{#if data.isUsersFavourite}
	<button class="remove-favourite" name="id" value={show._id} form="remove-favourite"
		>Remove from favorites</button
	>
{:else}
	<button class="add-favourite" name="id" value={show._id} form="add-favourite"
		>Add to favorites</button
	>
{/if}

{#await data.similarShows then smilarShows}
	{#if smilarShows}
		<h2>Similar shows</h2>
		<div class="similar-shows">
			{#each smilarShows as show}
				<ShowThumbnail {show} />
			{/each}
		</div>
	{/if}
{/await}

<style>

    .trailer-card {
        display: flex;
        flex-direction: column;
        align-items: center;
        background-color: var(--background-color);
        padding: 1rem;
        border-radius: 10px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        margin-bottom: 20px;
        margin-top: 20px;
    }

    .show-details {
        display: flex;
        align-items: flex-start;
        gap: 1rem;
        background-color: var(--background-color);
        padding: 1rem;
        border-radius: 10px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        margin-bottom: 20px;
        margin-top: 20px;
    }

    .thumbnail {
        max-width: 200px;
        border-radius: 10px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        margin-top: 20px;
    }

    .show-details p {
        color: var(--text-color);
        flex: 1;
    }

    .data-grid {
        display: grid;
        grid-template-columns: 1fr 2fr;
        gap: 1rem;
        background-color: var(--background-color);
        padding: 1rem;
        border-radius: 10px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    .data-grid .label {
        font-weight: bold;
        color: var(--text-color);
    }

    .data-grid span {
        color: var(--text-color);
    }

    button.add-favourite, button.remove-favourite {
        background-color: var(--primary-color);
        color: var(--text-button-color);
        padding: 10px;
        margin-top: 10px;
        cursor: pointer;
        border: none;
        border-radius: 5px;
        transition: background-color 0.3s ease;
    }

    button.add-favourite:hover, button.remove-favourite:hover {
        background-color: var(--primary-color-hover);
    }

    .similar-shows {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
        gap: 20px;
    }
</style>
