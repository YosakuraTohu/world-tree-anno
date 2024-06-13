<script lang="ts">
	import WtaMonth from './wta_month.svelte';

	export let kal: typeof import('kitten-anno-lib');
	export let now = kal.Anno.default();

	$: caom = (month: number) =>
		kal.Anno.from_time(BigInt(now.year.number), BigInt(month), 1n, 0n, 0n, 0n);
	$: craom = caom(1);
	$: is_common = craom.is_common.year;
	$: title = now.to_string();
	$: months = Array.from({ length: 27 + (!is_common ? 1 : 0) }, (_, k) => caom(k + 1));
</script>

<div class="year-wrapper">
	<h1>{title}</h1>
	<div class="year">
		{#each months as aom}
			<WtaMonth {aom} {now} />
		{/each}
	</div>
</div>

<style>
	.year-wrapper {
		margin: 0.5em 1em;

		backdrop-filter: blur(10px);
		-webkit-backdrop-filter: blur(10px);

		border: 1px solid black;
		border-radius: 0.5em;
	}

	.year-wrapper > h1 {
		text-align: center;
	}

	.year {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}
</style>
