<script lang="ts">
	import '../app.css';
	import { page } from '$app/state';
	import { fade, fly } from 'svelte/transition';
	import { cubicOut } from 'svelte/easing';

	let { children } = $props();

	let currentPath = $derived(page.url.pathname);

	$effect(() => {
		currentPath = page.url.pathname;
	});
</script>

<div class="app">
	{#key currentPath}
		<main
			in:fly={{ y: 20, duration: 500, delay: 300, easing: cubicOut }}
			out:fade={{ duration: 300 }}
		>
			{@render children()}
		</main>
	{/key}
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}
</style>
