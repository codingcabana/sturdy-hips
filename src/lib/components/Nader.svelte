<script lang="ts">

	let clicks = 0;

	const incrementClicks = () => {
		clicks += 1;
	};
</script>

<div>
	<label for="lang" class="w-24">
		<h3>{clicks}</h3>
		
		<!-- <select
			bind:value={lang}
			on:change={requestSubmit}
			id="lang"
			name="lang"
			class="block w-full rounded-lg border border-neutral-300 bg-white px-2 py-1 text-sm text-neutral-900 focus:border-indigo-500 focus:ring-indigo-500 dark:border dark:border-neutral-700 dark:bg-neutral-900 dark:text-white dark:placeholder-neutral-400 dark:focus:ring-indigo-500"
		>
			{#each languages as lang}
				<option selected={lang === language} value={lang}>{i(`locales.${lang}`)}</option>
			{/each}
		</select> -->
	</label>
	<button
		class="rounded-md border border-indigo-600 bg-indigo-500 p-1 text-xs font-medium text-white dark:border-indigo-600 dark:bg-indigo-500 dark:text-white"
		title={"Add Click"}
		on:click={incrementClicks}
	>
	Add Click!
	<!-- <ArrowPathIcon width="20" height="20" stroke-width="1.75" aria-hidden /> -->
	</button>
</div>
