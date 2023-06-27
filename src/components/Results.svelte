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
		const phonetic = data?.phonetics?.find((el) => !!el.text)!.text;

		return data.phonetic || phonetic;
	}
</script>

<section>
	<div>
		<h3>
			{wordData.word}
		</h3>

		<button class="audio-btn" on:click={() => playSound(wordData.phonetics)}>
			<img src="/audio.svg" alt="audio icon" />
		</button>
	</div>

	<h4 style="color: brown;">
		{phoneticString(wordData)}
	</h4>

	{#each wordData.meanings as meaning}
		<Definitions {meaning} />
	{/each}

	<div>
		<span>Source: </span>
		<a href={wordData.sourceUrls[0]}>{wordData.sourceUrls[0]}</a>
	</div>
</section>

<style>
	img {
		width: 50px;
	}

	.audio-btn {
		cursor: pointer;
	}
</style>
