<script lang="ts">
	import type { Meanings } from '../models/response.svelte';

	export let meaning: Meanings;
</script>

<h4>
	{meaning.partOfSpeech}
</h4>

<ul style="list-style: none;">
	{#each meaning.definitions as definition}
		<li style="margin: 1rem 0">{definition.definition}</li>
	{/each}
</ul>
