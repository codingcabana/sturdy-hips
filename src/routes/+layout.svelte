<script lang="ts">
	import '../app.postcss';

	import Header from '$components/Header.svelte';
	import SvelteTheme from '$components/SvelteThemes/SvelteTheme.svelte';

	export let data;
</script>

<div class="px-20 py-20">
	<Header />

	<slot />
</div>
<SvelteTheme attribute="data-theme" />
