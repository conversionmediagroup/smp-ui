<script lang="ts">
	import { fly } from 'svelte/transition';
	import { Check } from 'lucide-svelte';

	export let currentStep: number;
	export let steps: { id: string; title: string }[];
	export let progressPercentage: number;
	export let goToStep: (index: number) => void;
	export let visible: boolean;
	export let showWelcome: boolean;
	export let formSubmitted: boolean = false; // Add this prop to track form submission
</script>

{#if visible && !showWelcome && currentStep > 0}
	<div in:fly={{ y: 20, duration: 600 }} class="mx-auto mb-10 max-w-[1200px]">
		<div class="mb-2 flex items-center justify-between">
			<span class="text-sm font-medium text-gray-500">Step {currentStep} of {steps.length - 1}</span
			>
			<span class="text-sm font-medium text-blue-600"
				>{Math.round(progressPercentage)}% Complete</span
			>
		</div>

		<div class="h-2 w-full overflow-hidden rounded-full bg-gray-200">
			<div
				class="h-full rounded-full bg-blue-600 transition-all duration-500 ease-out"
				style="width: {formSubmitted ? '100' : progressPercentage}%"
			></div>
		</div>

		<div class="mt-6 hidden justify-between md:flex">
			{#each steps.slice(1) as step, index}
				<button on:click={() => goToStep(index + 1)} class="group flex flex-col items-center">
					<div
						class="relative mb-2 flex h-10 w-10 items-center justify-center rounded-full border-2 transition-all duration-200 {index +
							1 <
							currentStep ||
						(formSubmitted && index + 1 === steps.length - 1)
							? 'border-blue-600 bg-blue-600 text-white'
							: index + 1 === currentStep
								? 'border-blue-600 bg-white text-blue-600'
								: 'border-gray-300 bg-white text-gray-400'}"
					>
						{#if index + 1 < currentStep || (formSubmitted && index + 1 === steps.length - 1)}
							<Check size={18} />
						{:else}
							<span>{index + 1}</span>
						{/if}

						{#if index < steps.length - 2}
							<div
								class="absolute top-1/2 left-10 h-[2px] w-12 -translate-y-1/2 bg-gray-300 md:w-24 lg:w-32 {index +
									1 <
									currentStep || formSubmitted
									? 'bg-blue-600'
									: 'bg-gray-300'}"
							></div>
						{/if}
					</div>
					<span
						class="text-center text-xs font-medium {index + 1 <= currentStep || formSubmitted
							? 'text-blue-600'
							: 'text-gray-500'}">{step.title}</span
					>
				</button>
			{/each}
		</div>
	</div>
{/if}
