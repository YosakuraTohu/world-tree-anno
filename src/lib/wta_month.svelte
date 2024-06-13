<script lang="ts">
	import WtaDay from './wta_day.svelte';

	export let aom: import('kitten-anno-lib').Anno;
	export let now: import('kitten-anno-lib').Anno;

	$: offset = aom.chord.number - 1;
	$: is_common = aom.is_common.month;
	$: on = aom.month.number == now.month.number ? now.day.number : 0;
	$: title = aom.month.str + (!is_common ? '（大）' : '');
	$: days = Array.from({ length: 20 + offset + (!is_common ? 1 : 0) }, (_, k) => {
		let day = k + 1 - offset;
		let od = day == on && day != 0;
		return { day, od };
	});
</script>

<div class="month-wrapper" data-state={on != 0}>
	<h3>{title}</h3>
	<div class="month">
		{#each days as { day, od }}
			<WtaDay {day} {is_common} {od} />
		{/each}
	</div>
</div>

<style>
	.month-wrapper {
		margin: 0.5em 1em;

		border: 1px solid black;
		border-radius: 0.5em;
	}

	.month-wrapper[data-state='true'] > h3 {
		color: cornflowerblue;
	}

	.month-wrapper > h3 {
		margin-left: 0.5em;
	}

	.month {
		display: grid;
		grid-template: repeat(4, 2em) / repeat(9, 2em);
		gap: 0.5em;

		padding: 0.5em;
		border-top: 1px solid black;
	}
</style>
