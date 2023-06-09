<script lang="ts">
	import type { Phonetics, Response } from '../models/response.svelte';
	import Header from '../components/Header.svelte';
	import Spinner from '../components/Spinner.svelte';

	let word: string;
	let data: Response[] = [];
	let isLoading: boolean = false;

	async function fetchWord(word: string): Promise<void> {
		const response = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${word}`);
		data = await response.json();
		isLoading = false;

		console.log(data);
	}

	function playSound(phonetics: Phonetics[]) {
		const url = phonetics.find(phonetic => phonetic.audio.length > 1);

		const a = new Audio(url?.audio);
		a.play();
	}

	function phoneticString(data: Response): string {
		const phonetic = data?.phonetics?.find(el => !!el.text)!.text;

		return data.phonetic || phonetic;
	}
</script>

<Header />

<div class="counter">
	<h1>Dictionary App</h1>

	<div style="display: flex; flex-direction: column; justify-content: center; align-items: center;">
		<input bind:value={word} />
		<button
			style="cursor: pointer; margin-top: 1rem;"
			on:click={() => {
				if (!!word) {
					isLoading = true;
					fetchWord(word);
				}
			}}
		>
			Search
		</button>
	</div>

	{#if isLoading}
		<Spinner />
	{/if}

	{#if !isLoading}
		{#each data as wordData}
			<hr style="width: 100%" />

			<div>
				<div style="width: 100%; display: flex; justify-content: space-between;">
					<h3 style="font-size: 30px;">
						{wordData.word}
					</h3>

					<button style="font-size: 20px;" class="audio-btn" on:click={() => playSound(wordData.phonetics)}>
						<img src="/audio.svg" alt="audio icon" />
					</button>
				</div>

				<h4 style="color: brown;">
					{phoneticString(wordData)}
				</h4>

				{#each wordData.meanings as meaning}
					<h4>
						{meaning.partOfSpeech}
					</h4>

					<ul style="list-style: none;">
						{#each meaning.definitions as definition}
							<li style="margin: 1rem 0">{definition.definition}</li>
						{/each}
					</ul>
				{/each}

				<div style="margin: 2rem 0">
					<span style="font-weight: bold;">Source:</span>
					<a href={wordData.sourceUrls[0]}>{wordData.sourceUrls[0]}</a>
				</div>
			</div>
		{/each}
	{/if}
</div>

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

	img {
		width: 50px;
	}

	.counter {
		display: flex;
		flex-direction: column;
		border-bottom: 1px solid rgba(0, 0, 0, 0.1);
		margin: 1rem 0;
	}

	.counter button {
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
		margin-bottom: 1rem;
	}

	.counter button:hover {
		background-color: var(--color-bg-1);
	}

	.audio-btn {
		cursor: pointer;
	}
</style>
