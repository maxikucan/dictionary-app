<script lang="ts">
	import type { Meanings } from '../models/response.svelte';

	export let meaning: Meanings;
</script>

<div class="speech">
	<h4>
		{meaning.partOfSpeech}
	</h4>

	<span />
</div>

<ul>
	{#each meaning.definitions as definition}
		<li style="margin: 1rem 0">{definition.definition}</li>
	{/each}
</ul>

<style>
	ul {
		margin-left: 10px;
	}

	li {
		font-style: italic;
	}

	.speech {
		position: relative;
		display: flex;
		align-items: center;
	}

	span  {
		width: 90%;
		height: 1px;
		margin-left: 15px;
		margin-top: 1px;
		background-color: var(--tertiary);
	}
</style>
