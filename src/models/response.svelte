<script context="module" lang="ts">
	export interface Response {
		word: string;
		phonetic?: string;
		phonetics: Phonetics[];
		meanings: Meanings[];
		license: License;
		sourceUrls: string[];
	}

	export interface Phonetics {
		text: string;
		audio: string;
		sourceUrl?: string;
	}

	export interface Meanings {
		partOfSpeech: string;
		definitions: Definition[];
	}

	export interface Definition {
		definition: string;
		synonyms: string[];
		antonyms: string[];
		example: string;
	}

	export interface License {
		name: string;
		url: string;
	}
</script>
