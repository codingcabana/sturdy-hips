<script lang="ts">
	import { language } from '@inlang/sdk-js';

	import LocaleSwitcher from '$components/LocaleSwitcher.svelte';
	import ThemeSwitcher from '$components/ThemeSwitcher.svelte';
	import Spinner from '$components/Spinner.svelte';

	import Nader from '$components/Nader.svelte';
</script>

<header>
	<div>
		<a href="/{language}">
			<h1>Home</h1>
		</a>

		<Nader />

		<LocaleSwitcher />

		<ThemeSwitcher />

		<Spinner />
	</div>
</header>
