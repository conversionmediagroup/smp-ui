<script lang="ts">
	import { fly } from 'svelte/transition';
	import { GraduationCap, ChevronRight } from 'lucide-svelte';

	export let startForm: () => void;
</script>

<div in:fly={{ y: 20, duration: 300, delay: 200 }} class="w-full py-8 text-center">
	<div class="mx-auto mb-6 flex h-24 w-24 items-center justify-center rounded-full bg-blue-100">
		<GraduationCap size={48} class="text-blue-600" />
	</div>
	<h3 class="mb-4 text-2xl font-bold text-gray-900">Welcome to School Match Pro</h3>
	<p class="mx-auto mb-8 max-w-lg text-gray-600">
		We're excited to help you find the perfect educational path. This quick questionnaire will help
		us understand your needs and match you with ideal programs.
	</p>
	<button
		on:click={startForm}
		class="inline-flex cursor-pointer items-center justify-center rounded-lg bg-blue-600 px-8 py-3 text-base font-medium text-white shadow-md transition-all hover:bg-blue-700 hover:shadow-lg focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:outline-none"
	>
		Get Started
		<ChevronRight size={18} class="ml-2" />
	</button>
</div>
