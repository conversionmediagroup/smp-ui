<script lang="ts">
	import { fade } from 'svelte/transition';
	import { Shield, Check, Sparkles } from 'lucide-svelte';

	export let visible: boolean;
	export let formSubmitted: boolean;
</script>

{#if visible && !formSubmitted}
	<div
		in:fade={{ duration: 800, delay: 400 }}
		class="mx-auto mt-8 max-w-[1200px] rounded-xl border border-gray-200 bg-gray-50 p-6"
	>
		<div
			class="flex flex-col items-center justify-between gap-4 text-center md:flex-row md:text-left"
		>
			<div class="flex items-center">
				<Shield size={20} class="mr-2 text-blue-600" />
				<span class="text-sm font-medium text-gray-700"
					>Your information is secure and encrypted</span
				>
			</div>
			<div class="flex items-center">
				<Check size={20} class="mr-2 text-blue-600" />
				<span class="text-sm font-medium text-gray-700">100% satisfaction guarantee</span>
			</div>
			<div class="flex items-center">
				<Sparkles size={20} class="mr-2 text-blue-600" />
				<span class="text-sm font-medium text-gray-700"
					>Matched with accredited institutions only</span
				>
			</div>
		</div>
	</div>
{/if}
