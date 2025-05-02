<script lang="ts">
	import { fade } from 'svelte/transition';
	import { GraduationCap } from 'lucide-svelte';

	export let visible: boolean;
	export let formSubmitted: boolean;
</script>

{#if visible && !formSubmitted}
	<div in:fade={{ duration: 800 }} class="mb-8 text-center">
		<div
			class="mx-auto mb-4 inline-flex items-center justify-center rounded-full bg-blue-100 px-4 py-2 text-blue-700"
		>
			<GraduationCap size={18} class="mr-2" />
			<span class="text-sm font-semibold">Find Your Perfect School Match</span>
		</div>
		<h2 class="mb-4 text-3xl font-bold tracking-tight text-gray-900 md:text-4xl">
			Your Educational Journey <span class="text-blue-600">Starts Here</span>
		</h2>
		<p class="mx-auto max-w-2xl text-lg text-gray-600">
			Complete this short form and we'll match you with schools that align with your goals,
			preferences, and academic background.
		</p>
	</div>
{/if}
