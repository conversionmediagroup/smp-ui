<script lang="ts">
	import { onMount } from 'svelte';
	import ImageCarousel from './ImageCarousel.svelte';
	import { goto } from '$app/navigation';
	import { fade, fly, scale, draw, crossfade } from 'svelte/transition';
	import { cubicOut, elasticOut, bounceOut } from 'svelte/easing';
	import { tweened } from 'svelte/motion';
	import Logo from '../../../../static/assets/smp-logo.svg';
	import SmpLogo from '../Header/SMPLogo.svelte';

	let isLoaded = false;
	let isNavigating = false;
	let progress = tweened(0, { duration: 800, easing: cubicOut });
	let particles = Array(50)
		.fill()
		.map(() => ({
			x: Math.random() * 100,
			y: Math.random() * 100,
			size: Math.random() * 10 + 5,
			speed: Math.random() * 2 + 1,
			delay: Math.random() * 500,
			opacity: Math.random() * 0.7 + 0.3
		}));

	// For the animated path
	let pathProgress = 0;
	let pathInterval;

	onMount(() => {
		setTimeout(() => {
			isLoaded = true;
		}, 300);
	});

	// Handle navigation with transition
	async function handleGetStarted() {
		// Start the transition
		isNavigating = true;

		// Animate progress
		progress.set(1);

		// Animate path
		pathInterval = setInterval(() => {
			pathProgress = (pathProgress + 0.01) % 1;
		}, 20);

		// Wait for transition to complete before navigation
		setTimeout(() => {
			clearInterval(pathInterval);
			goto('/match', { replaceState: false });
		}, 2500);
	}
</script>

{#if isNavigating}
	<div
		transition:fade={{ duration: 300, easing: cubicOut }}
		class="fixed inset-0 z-50 overflow-hidden bg-gradient-to-br from-blue-900 via-indigo-800 to-blue-700"
	>
		<!-- Particles -->
		{#each particles as particle, i}
			<div
				class="absolute rounded-full bg-white"
				style="
					left: {particle.x}%;
					top: {particle.y}%;
					width: {particle.size}px;
					height: {particle.size}px;
					opacity: {particle.opacity};
					transform: scale({$progress * 1.5});
					transition: transform {800 + particle.delay}ms cubic-bezier(0.34, 1.56, 0.64, 1);
				"
			></div>
		{/each}

		<div class="absolute inset-0 flex flex-col items-center justify-center">
			<!-- Company Logo -->
			<div
				in:scale={{ duration: 800, delay: 200, easing: elasticOut, start: 0.5 }}
				class="mb-6 scale-150"
			>
				<SmpLogo fillColor="white" />
			</div>

			<div class="relative h-12 overflow-hidden">
				<div class="flex justify-center">
					{#each 'Your perfect school awaits you...'.split('') as char, i}
						<span
							in:fly={{ y: 40, delay: 300 + i * 50, duration: 800, easing: elasticOut }}
							class="text-3xl font-bold text-white"
						>
							{char === ' ' ? '\u00A0' : char}
						</span>
					{/each}
				</div>
			</div>

			<!-- Animated progress bar -->
			<div class="mt-12 h-2 w-64 overflow-hidden rounded-full bg-white/20">
				<div
					class="h-full rounded-full bg-white"
					style="width: {$progress * 100}%; transition: width 2s cubic-bezier(0.34, 1.56, 0.64, 1);"
				></div>
			</div>

			<!-- Animated path with gradient -->
			<svg width="300" height="80" viewBox="0 0 300 80" class="mt-8 opacity-70">
				<defs>
					<linearGradient id="gradient" x1="0%" y1="0%" x2="100%" y2="0%">
						<stop offset="0%" stop-color="#4F46E5" />
						<stop offset="100%" stop-color="#06B6D4" />
					</linearGradient>

					<mask id="mask">
						<path
							d="M10,40 C50,0 100,80 150,40 C200,0 250,80 290,40"
							stroke="white"
							stroke-width="8"
							fill="none"
							stroke-dasharray="300"
							stroke-dashoffset={300 - pathProgress * 300}
						/>
					</mask>
				</defs>

				<rect x="0" y="0" width="300" height="80" fill="url(#gradient)" mask="url(#mask)" />
			</svg>

			<!-- Animated dots -->
			<div class="mt-4 flex space-x-2">
				{#each [0, 1, 2] as i}
					<div
						class="h-3 w-3 rounded-full bg-white"
						style="animation: bounce 1s {i * 0.2}s infinite alternate;"
					></div>
				{/each}
			</div>
		</div>

		<!-- Radial gradient overlay -->
		<div
			class="bg-radial-gradient pointer-events-none absolute inset-0"
			style="background: radial-gradient(circle, transparent 30%, rgba(0,0,0,0.3) 100%);"
		></div>
	</div>
{/if}

<section
	class="relative overflow-hidden bg-gradient-to-br from-blue-50 to-indigo-50 py-12 lg:min-h-[700px] lg:py-20"
>
	<div class="absolute top-20 left-10 h-64 w-64 rounded-full bg-blue-300/10 blur-3xl"></div>
	<div class="absolute right-10 bottom-10 h-80 w-80 rounded-full bg-indigo-300/10 blur-3xl"></div>

	<div class="relative z-10 container mx-auto px-4">
		<div class="flex flex-col items-center gap-8 lg:flex-row lg:items-center lg:gap-12">
			<div class="w-full space-y-8 text-center lg:w-1/2 lg:self-start lg:text-left">
				<div class="space-y-2">
					<h1
						class="xl-mobile:text-6xl transform text-[9.5vw] leading-[1.1] font-bold tracking-tighter text-slate-800 transition-all duration-700 md:leading-tight {isLoaded
							? 'translate-y-0 opacity-100'
							: 'translate-y-4 opacity-0'}"
					>
						Find Your Perfect School with <span class="text-blue-600">School Match Pro</span>
					</h1>
				</div>

				<p
					class="xl-mobile:text-3xl transform text-[6vw] leading-tight tracking-tighter text-slate-700 transition-all duration-900 lg:text-4xl {isLoaded
						? 'translate-y-0 opacity-100'
						: 'translate-y-4 opacity-0'}"
				>
					Your gateway to a brighter future!
				</p>

				<p
					class="xl-mobile:text-xl transform text-[5vw] leading-relaxed text-slate-600 transition-all duration-1000 md:mx-auto md:w-[500px] md:text-xl lg:mx-[unset] {isLoaded
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
						class="loading w-[50vw] max-w-[200px] cursor-pointer rounded-full bg-gradient-to-r from-blue-600 to-indigo-500 py-1.5 text-white shadow-md transition-all duration-300 hover:scale-[1.02] sm:py-2 md:py-2.5 lg:mx-0"
					>
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
					class="relative mx-auto max-w-lg overflow-hidden rounded-2xl shadow-2xl lg:ml-auto lg:max-w-3xl"
				>
					<ImageCarousel />
				</div>
			</div>
		</div>
	</div>

	<!-- Wave decoration at bottom -->
	<div
		class="absolute right-0 bottom-0 left-0 h-16 -skew-y-2 transform bg-white/50 backdrop-blur-sm"
	></div>
</section>

<style>
	/* Add any additional custom styles here */
	@keyframes float {
		0% {
			transform: translateY(0px);
		}
		50% {
			transform: translateY(-10px);
		}
		100% {
			transform: translateY(0px);
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

	.animate-pulse {
		animation: pulse 1.5s cubic-bezier(0.4, 0, 0.6, 1) infinite;
	}
</style>
