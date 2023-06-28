<script lang="ts">
	import type { Response, ErrorResponse } from '../models/response.svelte';
	import Header from '../components/Header.svelte';
	import Spinner from '../components/Spinner.svelte';
	import Results from '../components/Results.svelte';

	let word: string;
	let data: Response[] = [];
	let error: ErrorResponse | null;
	let isLoading: boolean = false;

	async function fetchWord(word: string): Promise<void> {
		try {
			error = null;
			const response = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${word}`);
			data = await response.json();
			isLoading = false;

			if (!Array.isArray(data)) {
				error = data;
			}
		} catch (e) {
			console.error(e);
		}
	}
</script>

<Header />

<main class="app-container">
	<section style="display: flex; flex-direction: column; justify-content: center; align-items: center;">
		<form
			on:submit={(e) => {
				e.preventDefault();

				if (!!word) {
					isLoading = true;
					fetchWord(word);
				}
			}}
		>
			<div class="search">
				<input bind:value={word} placeholder="Search for a word..." />

				<button class="search-btn">
					<img src="/search.svg" alt="search icon" width="30px" />
				</button>
			</div>
		</form>
	</section>

	{#if isLoading}
		<Spinner />
	{/if}

	{#if !isLoading}
		{#if !!error}
			<div class="error-text">
				<h3>{error.title}</h3>
				<p>{error.message}</p>
			</div>
		{/if}

		{#if !error}
			{#each data as wordData}
				<Results {wordData} />
			{/each}
		{/if}
	{/if}
</main>

<style>
	@import '../reset.css';
	:root {
		font-family: 'Roboto', sans-serif;
	}
	.app-container {
		min-height: 80vh;
		width: 100vw;
		display: flex;
		flex-direction: column;
		justify-content: center;
	}

	.search {
		display: flex;
		align-items: center;
		gap: 1rem;
		padding: 0 10px;
	}

	input {
		height: 3rem;
		width: 90%;
		background-color: #2e2e2e0e;
		outline: none;
		border: solid 1px #0000001a;
		border-radius: 8px;
		font-size: 30px;
		padding-left: 8px;
	}

	input:focus {
		border: solid 1px #0000003a;
	}

	.search-btn {
		min-width: 2em;
		height: 3rem;
		padding: 5px;
		border-radius: 8px;
		display: flex;
		align-items: center;
		justify-content: center;
		border: solid 1px #2e2e2e4b;
		background-color: var(--primary);
		touch-action: manipulation;
		font-size: 2rem;
		margin: 1rem 0;
		cursor: pointer;
	}

	.search-btn:hover {
		background-color: #5e5e5e10;
	}

	.error-text {
		display: flex;
		flex-direction: column;
		justify-content: center;
		width: 40%;
		margin: 1rem auto;
	}

	@media (min-width: 980px) {
		.search {
			min-width: 50rem;
		}
	}
</style>
