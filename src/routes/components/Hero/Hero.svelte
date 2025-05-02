<script lang="ts">
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	import ImageCarousel from './ImageCarousel.svelte';
	import LoadingScreen from './LoadingScreen.svelte';

	let isLoaded = false;
	let isNavigating = false;

	onMount(() => {
		setTimeout(() => {
			isLoaded = true;
		}, 300);
	});

	async function handleGetStarted() {
		isNavigating = true;
	}
</script>

{#if isNavigating}
	<LoadingScreen onComplete={() => goto('/match', { replaceState: false })} />
{/if}

<section
	class="relative overflow-hidden bg-gradient-to-br from-sky-700 to-blue-900 py-12 lg:min-h-[700px] lg:py-20"
>
	<div class="absolute top-20 left-10 h-64 w-64 rounded-full bg-blue-400/20 blur-3xl"></div>
	<div class="absolute right-10 bottom-10 h-80 w-80 rounded-full bg-sky-400/20 blur-3xl"></div>
	<div class="absolute top-1/2 left-1/3 h-40 w-40 rounded-full bg-cyan-300/10 blur-2xl"></div>

	<div class="absolute inset-0 overflow-hidden">
		{#each Array(8) as _, i}
			<div
				class="absolute rounded-full bg-blue-300/10"
				style="
					width: {Math.random() * 300 + 100}px;
					height: {Math.random() * 300 + 100}px;
					left: {Math.random() * 100}%;
					top: {Math.random() * 100}%;
					animation: float {Math.random() * 10 + 15}s infinite ease-in-out;
				"
			></div>
		{/each}
	</div>

	<div class="relative z-10 container mx-auto px-4">
		<div class="flex flex-col items-center gap-8 lg:flex-row lg:items-center lg:gap-12">
			<div class="w-full space-y-8 text-center lg:w-1/2 lg:self-start lg:text-left">
				<div class="space-y-2">
					<h1
						class="xl-mobile:text-6xl transform text-[9.5vw] leading-[1.1] font-bold tracking-tighter text-white transition-all duration-700 md:text-5xl md:leading-tight lg:text-6xl {isLoaded
							? 'translate-y-0 opacity-100'
							: 'translate-y-4 opacity-0'}"
					>
						Find Your Perfect School with <span class="text-sky-300">School Match Pro</span>
					</h1>
				</div>

				<p
					class="xl-mobile:text-3xl transform text-[6vw] leading-tight tracking-tighter text-blue-100 transition-all duration-900 md:text-2xl lg:text-4xl {isLoaded
						? 'translate-y-0 opacity-100'
						: 'translate-y-4 opacity-0'}"
				>
					Your gateway to a brighter future!
				</p>

				<p
					class="xl-mobile:text-xl transform text-[5vw] leading-relaxed text-blue-200 transition-all duration-1000 md:mx-auto md:w-[500px] md:text-xl lg:mx-[unset] {isLoaded
						? 'translate-y-0 opacity-100'
						: 'translate-y-4 opacity-0'}"
				>
					Connect with top accredited colleges and universities that match your educational goals
					and preferences.
				</p>

				<div
					class="transform transition-all duration-1100 {isLoaded
						? 'translate-y-0 opacity-100'
						: 'translate-y-4 opacity-0'}"
				>
					<button
						on:click={handleGetStarted}
						class="loading group relative w-[50vw] max-w-[200px] cursor-pointer overflow-hidden rounded-full bg-gradient-to-r from-sky-400 to-blue-500 py-1.5 text-white shadow-lg transition-all duration-300 hover:scale-[1.02] hover:shadow-blue-300/30 sm:py-2 md:py-2.5 lg:mx-0"
					>
						<span
							class="group-hover:animate-shine absolute inset-0 h-full w-full -translate-x-full bg-gradient-to-r from-transparent via-white/20 to-transparent"
						></span>

						<span class="relative z-10 text-sm font-medium text-white sm:text-base md:text-lg">
							Get Started
						</span>
					</button>
				</div>
			</div>

			<div
				class="w-full transform transition-all duration-1200 lg:w-1/2 {isLoaded
					? 'translate-y-0 opacity-100'
					: 'translate-y-8 opacity-0'}"
			>
				<div
					class="relative mx-auto max-w-lg overflow-hidden rounded-2xl border border-white shadow-2xl backdrop-blur-sm lg:ml-auto lg:max-w-3xl"
				>
					<div
						class="pointer-events-none absolute inset-0 z-10 bg-gradient-to-br from-blue-500/10 to-transparent"
					></div>
					<ImageCarousel />
				</div>
			</div>
		</div>
	</div>

	<!-- Wave decoration at bottom -->
	<div class="absolute right-0 bottom-0 left-0">
		<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320" class="h-auto w-full">
			<path
				fill="#ffffff"
				fill-opacity="0.2"
				d="M0,128L48,144C96,160,192,192,288,186.7C384,181,480,139,576,138.7C672,139,768,181,864,181.3C960,181,1056,139,1152,122.7C1248,107,1344,117,1392,122.7L1440,128L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z"
			></path>
		</svg>
	</div>
</section>

<style>
	@keyframes float {
		0% {
			transform: translateY(0px) rotate(0deg);
		}
		50% {
			transform: translateY(-20px) rotate(5deg);
		}
		100% {
			transform: translateY(0px) rotate(0deg);
		}
	}

	@keyframes pulse {
		0%,
		100% {
			opacity: 1;
		}
		50% {
			opacity: 0.7;
		}
	}

	@keyframes bounce {
		0% {
			transform: translateY(0);
		}
		100% {
			transform: translateY(-10px);
		}
	}

	@keyframes shine {
		100% {
			transform: translateX(100%);
		}
	}

	.animate-pulse {
		animation: pulse 1.5s cubic-bezier(0.4, 0, 0.6, 1) infinite;
	}

	.animate-shine {
		animation: shine 1.5s ease-in-out infinite;
	}

	.backdrop-blur-sm {
		backdrop-filter: blur(4px);
	}
</style>
