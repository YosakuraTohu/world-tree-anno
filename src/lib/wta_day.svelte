<script lang="ts">
	export let day: number;
	export let is_common: boolean;
	export let od: boolean;

	export let checked = false;

	$: state = (day > 0 && day <= 20 + (!is_common ? 1 : 0) ? 1 : 0) + (od ? 1 : 0);

	function handleClick(event: MouseEvent | KeyboardEvent) {
		const target = event.target as HTMLDivElement;

		const state = target.getAttribute('data-checked');

		checked = state === 'true' ? false : true;
	}
</script>

<div
	class="day"
	tabindex="0"
	role="button"
	aria-pressed="false"
	data-state={state}
	data-checked={checked}
	on:click={handleClick}
	on:keydown={handleClick}
>
	<span>{day}</span>
</div>

<style>
	.day {
		position: relative;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.day[data-state='0'] {
		visibility: hidden;
	}

	.day[data-state='2'] {
		color: red;
	}

	.day[data-checked='false'] {
		background: unset;
	}

	.day[data-checked='true'] {
		background: red;
	}

	.day::before {
		content: '';

		position: absolute;
		display: block;
		top: 0;
		left: 0;

		height: 19.1%;
		width: 19.1%;

		border-top: 1px solid #00000080;
		border-left: 1px solid #00000080;
	}

	.day::after {
		content: '';

		position: absolute;
		display: block;
		bottom: 0;
		right: 0;

		height: 19.1%;
		width: 19.1%;

		border-bottom: 1px solid #00000080;
		border-right: 1px solid #00000080;
	}

	.day:hover::before {
		top: unset;
		bottom: 0;

		border-top: unset;
		border-bottom: 1px solid #00000080;
	}

	.day:hover::after {
		bottom: unset;
		top: 0;

		border-bottom: unset;
		border-top: 1px solid #00000080;
	}
</style>
