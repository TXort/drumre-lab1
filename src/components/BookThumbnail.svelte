<script lang="ts">
	import { enhance } from '$app/forms';
	import type { BookType } from '$lib/types';
	import type { Snippet } from 'svelte';

	let { book, children, showDeleteButton }: { book: BookType; children?: Snippet; showDeleteButton?: boolean } = $props();
</script>

<article>
	<form method="POST" action="?/delete" use:enhance id="delete"></form>
	<form method="POST" action="/books?/removeFavourite" use:enhance id="remove-favourite"></form>
	<form method="POST" action="/books?/addFavourite" use:enhance id="add-favourite"></form>

	<div class="book-container">
		<a href={`/books/${book._id}`}>
			<img src={book.imageLinks?.thumbnail || '/book-cover-placeholder.jpg'}
					 alt={book.title}
					 class="book-thumbnail" />
		</a>

		<div class="book-info">
			<h2>
				<a>
					{book.title}
				</a>
			</h2>
			<p>{book.authors}</p>
		</div>

		<div class="buttons {showDeleteButton ? '': 'single-button'}">
			{#if book.isFavourite}
				<button class="remove-favourite" name="id" value={book._id} form="remove-favourite"
			>Remove from favorites</button
			>
		{:else}
			<button class="add-favourite" name="id" value={book._id} form="add-favourite"
			>Add to favorites</button
			>
		{/if}

		{#if showDeleteButton}
			<button class="delete" name="id" value={book._id} form="delete">Delete</button>
		{/if}

	</div>
	</div>



	{#if children}
		{@render children()}
	{/if}
</article>

<style>


    article {
        border: 1px solid transparent;
        border-radius: 10px;
        overflow: hidden;
        text-align: center;
    }
    .book-container {
        max-width: 200px;
    }

		/* IMAGE */
    img.book-thumbnail {
        width: 100%;
        height: auto;
        display: block;
        border: 1px solid var(--border-color);
        border-radius: 10px;
    }

		/* BUTTONS */
    .buttons {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .buttons.single-button {
        justify-content: center;
    }

		button:hover {
			filter: brightness(0.8);
    }

    button.add-favourite, button.remove-favourite, button.delete {
        background-color: var(--primary-color);
        color: var(--text-button-color);
        height: 40px;
        cursor: pointer;
        border: none;
        border-radius: 5px;
    }

    button.delete, button.remove-favourite {
        background-color: var(--danger-color);
    }

    /*
    BOOK INFO
    https://stackoverflow.com/questions/3922739/limit-text-length-to-n-lines-using-css
     */
    .book-info {
        width: 100%;
        color: var(--text-color);
    }

    .book-info h2 {
        overflow: hidden;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        line-clamp: 2;
        -webkit-box-orient: vertical;
    }

		.book-info p {
				filter: brightness(0.8);
		}


</style>