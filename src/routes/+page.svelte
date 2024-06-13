<script lang="ts">
	import '$lib/background/background.css';
	import '$lib/font/kose.css';
	import 'normalize.css';
	import '$lib/css/breakpoint.css';
	import { onMount } from 'svelte';
	import WtaYear from '$lib/wta_year.svelte';
	import Caculator from '$lib/caculator.svelte';

	let kal: typeof import('kitten-anno-lib') | undefined;
	let now: import('kitten-anno-lib').Anno | undefined;

	onMount(async () => {
		kal = await import('kitten-anno-lib');

		setInterval(() => {
			now = kal?.Anno.get_anno();
		}, 8);
	});
</script>

{#if kal != undefined}
	<Caculator {kal} />
	<WtaYear {kal} {now} />
{/if}

<style>
	:global(html) {
		font-family: 'Kose';
	}
</style>
