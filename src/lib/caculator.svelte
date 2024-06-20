<script lang="ts">
	import type { Anno } from 'kitten-anno-lib';
	import { onMount } from 'svelte';

	export let kal: typeof import('kitten-anno-lib');

	$: ceValue = '2017-04-25T00:00';
	$: wtaYear = 1;
	$: wtaMonth = 1;
	$: wtaDay = 1;
	$: wtaHour = 0;
	$: wtaMinute = 0;
	$: wtaSecond = 0;
	$: wtaStr = '';

	function ceFlash(timestamp: number) {
		let date = new Date(timestamp * 1000);

		let format = (n: number) => {
			return n < 10 ? '0' + n : n.toString();
		};

		ceValue = `${date.getFullYear()}-${format(date.getMonth())}-${format(date.getDay())}T${format(date.getHours())}:${format(date.getMinutes())}`;
	}

	function checkValue() {
		wtaYear = (wtaYear ?? 1) >= 1 ? wtaYear : 1;
		wtaMonth = (wtaMonth ?? 1) >= 1 ? wtaMonth : 1;
		wtaDay = (wtaDay ?? 1) >= 1 ? wtaDay : 1;
		wtaHour = (wtaHour ?? 0) >= 0 ? wtaHour : 0;
		wtaMinute = (wtaMinute ?? 0) >= 0 ? wtaMinute : 0;
		wtaSecond = (wtaSecond ?? 0) >= 0 ? wtaSecond : 0;
	}

	function wtaFlash(anno: Anno) {
		wtaYear = Number(anno.year.number);
		wtaMonth = Number(anno.month.number);
		wtaDay = Number(anno.day.number);
		wtaHour = Number(anno.hms.hour);
		wtaMinute = Number(anno.hms.minute);
		wtaSecond = Number(anno.hms.second);
		wtaStr = anno.to_string();
	}

	function ceOnChange() {
		let ts = Date.parse(ceValue);
		let unixTime = Math.floor(ts / 1000);
		let wta = kal.Anno.from_common_era(BigInt(unixTime));
		wtaFlash(wta);
	}

	function wtaOnChange() {
		checkValue();
		let wta = kal.Anno.from_time(
			BigInt(wtaYear),
			BigInt(wtaMonth),
			BigInt(wtaDay),
			BigInt(wtaHour),
			BigInt(wtaMinute),
			BigInt(wtaSecond)
		);
		wtaFlash(wta);
		ceFlash(Number(wta.to_common_era()));
	}

	onMount(ceOnChange);
</script>

<div class="caculator-wrapper">
	<h1>世界树纪元计算器</h1>
	<div class="caculator">
		<div>
			<h3>公元</h3>
			<div class="inner">
				<input
					type="datetime-local"
					class="ce-e"
					name="ce"
					id="ce"
					min="2017-04-25T00:00"
					bind:value={ceValue}
					on:change={ceOnChange}
				/>
			</div>
		</div>
		<dev>
			<h3>世界树纪元</h3>
			<div class="inner inner-wta">
				<div class="wta-year">
					<label for="wta-year">年</label>
					<input
						type="number"
						class="wta-e"
						name="wta-year"
						id="wta-year"
						step="1"
						bind:value={wtaYear}
						on:change={wtaOnChange}
					/>
				</div>
				<div class="wta-month wta-s-in">
					<label for="wta-month">月</label>
					<input
						type="number"
						class="wta-e"
						name="wta-month"
						id="wta-month"
						step="1"
						bind:value={wtaMonth}
						on:change={wtaOnChange}
					/>
				</div>
				<div class="wta-day wta-s-in">
					<label for="wta-day">日</label>
					<input
						type="number"
						class="wta-e"
						name="wta-day"
						id="wta-day"
						step="1"
						bind:value={wtaDay}
						on:change={wtaOnChange}
					/>
				</div>
				<div class="wta-hour wta-s-in">
					<label for="wta-day">时</label>
					<input
						type="number"
						class="wta-e"
						name="wta-day"
						id="wta-day"
						step="1"
						bind:value={wtaHour}
						on:change={wtaOnChange}
					/>
				</div>
				<div class="wta-minute wta-s-in">
					<label for="wta-day">分</label>
					<input
						type="number"
						class="wta-e"
						name="wta-day"
						id="wta-day"
						step="1"
						bind:value={wtaMinute}
						on:change={wtaOnChange}
					/>
				</div>
				<div class="wta-second wta-s-in">
					<label for="wta-day">秒</label>
					<input
						type="number"
						class="wta-e"
						name="wta-day"
						id="wta-day"
						step="1"
						bind:value={wtaSecond}
						on:change={wtaOnChange}
					/>
				</div>
				<p class="wta-str">{wtaStr}</p>
			</div>
		</dev>
	</div>
</div>

<style>
	.caculator-wrapper {
		margin: 0.5em 1em;

		backdrop-filter: blur(10px);
		-webkit-backdrop-filter: blur(10px);

		border: 1px solid black;
		border-radius: 0.5em;
	}

	.caculator-wrapper > h1 {
		text-align: center;
	}

	.caculator {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}

	.caculator > * {
		margin: 0.5em 1em;

		width: 24em;

		border: 1px solid black;
		border-radius: 0.5em;
	}

	.caculator > * > h3 {
		margin-left: 0.5em;
	}

	.inner {
		padding: 0.5em;
		border-top: 1px solid black;
	}

	.inner-wta {
		display: grid;

		grid-template: repeat(3, 1fr) auto / repeat(6, 1fr);
	}

	.inner-wta > div {
		margin-bottom: 1em;
	}

	.ce-e {
		background: unset;

		border: 1px solid black;
		border-radius: 0.5em;
	}

	.wta-e {
		display: inline-block;

		background: unset;

		border: 1px solid black;
		border-radius: 0.5em;
	}

	.wta-year {
		grid-area: 1 / 1 / 2 / 7;
	}

	.wta-year > input {
		width: 16em;
	}

	.wta-month {
		grid-area: 2 / 1 / 3 / 3;
	}

	.wta-day {
		grid-area: 2 / 3 / 3 / 5;
	}

	.wta-hour {
		grid-area: 3 / 1 / 4 / 3;
	}

	.wta-minute {
		grid-area: 3 / 3 / 4 / 5;
	}

	.wta-second {
		grid-area: 3 / 5 / 4 / 7;
	}

	.wta-s-in > input {
		width: 4em;
	}

	.wta-str {
		grid-area: 4 / 1 / 5 / 7;
	}
</style>
