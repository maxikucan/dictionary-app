<script lang="ts">
	import type { Phonetics, Response } from '../models/response.svelte';
	import Definitions from './Definitions.svelte';

	export let wordData: Response;

	function playSound(phonetics: Phonetics[]) {
		const url = phonetics.find((phonetic) => phonetic.audio.length > 1);

		const a = new Audio(url?.audio);
		a.play();
	}

	function phoneticString(data: Response): string {
		let phonetic;

		if (data.phonetics.length) {
			phonetic = data?.phonetics?.find((el) => !!el.text)!.text;
		}

		return data.phonetic || phonetic || '';
	}
</script>

<section class="results-container">
	<div class="word-result">
		<h3>
			{wordData.word}
		</h3>

		<button class="audio-btn" on:click={() => playSound(wordData.phonetics)}>
			<img src="/audio.svg" alt="audio icon" />
		</button>
	</div>

	<h4>
		{phoneticString(wordData)}
	</h4>

	{#each wordData.meanings as meaning}
		<Definitions {meaning} />
	{/each}

	<div class="source">
		<hr style="margin: 1rem 0;"/>

		<span>Source: </span>
		<a href={wordData.sourceUrls[0]}>{wordData.sourceUrls[0]}</a>
	</div>
</section>

<style>
	.results-container {
		width: 95%;
		margin: 0 auto;
		padding: 10px 5px;
		border-bottom: solid 1px #343a4021;
	}

	.word-result {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-bottom: 1rem;
	}

	.word-result h3 {
		font-size: 25px;
	}

	h4 {
		color: brown;
		margin: 1rem 0;
	}

	img {
		width: 50px;
	}

	.audio-btn {
		cursor: pointer;
	}

	.source {
		margin: 1rem 0;
	}

	span {
		font-weight: bold;
	}

	@media (min-width: 1200px) {
		.results-container {
			width: 40%;
		}
	}
</style>
