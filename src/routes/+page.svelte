<script lang="ts">
	import type { Response, ErrorResponse } from '../models/response.svelte';
	import Header from '../components/Header.svelte';
	import Spinner from '../components/Spinner.svelte';
	import Results from '../components/Results.svelte';

	let word: string;
	let data: Response[] = [];
	let error: ErrorResponse;
	let isLoading: boolean = false;

	async function fetchWord(word: string): Promise<void> {
		try {
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
	<h1>Dictionary App</h1>

	<section style="display: flex; flex-direction: column; justify-content: center; align-items: center;">
		<input bind:value={word} />
		<button
			class="search-btn"
			on:click={() => {
				if (!!word) {
					isLoading = true;
					fetchWord(word);
				}
			}}
		>
			Search
		</button>
	</section>

	{#if isLoading}
		<Spinner />
	{/if}

	{#if !isLoading}
		{#if !!error}
			<h3>{error.title}</h3>
			<p>{error.message}</p>
		{/if}

		{#if !error}
			{#each data as wordData}
				<Results {wordData} />
			{/each}
		{/if}
	{/if}
</main>

<style>
	:root {
		font-family: 'Roboto', sans-serif;
	}

	h1 {
		text-align: center;
	}

	input {
		height: 2rem;
		min-width: 50vw;
	}

	.app-container {
		min-height: 80vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin: 1rem 0;
	}

	.search-btn {
		min-width: 2em;
		padding: 5px;
		border-radius: 8px;
		display: flex;
		align-items: center;
		justify-content: center;
		border: solid 1px #2e2e2e4b;
		background-color: transparent;
		touch-action: manipulation;
		font-size: 2rem;
		margin: 1rem 0;
		cursor: pointer;
	}

	.search-btn:hover {
		background-color: #5e5e5e10;
	}
</style>
