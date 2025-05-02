<script lang="ts">
	import { fade, fly, scale } from 'svelte/transition';
	import { cubicOut, elasticOut } from 'svelte/easing';
	import { tweened } from 'svelte/motion';
	import SmpLogo from '../Header/SMPLogo.svelte';

	// Props for customization
	export let onComplete: () => void = () => {};

	// State for animation (identical to original)
	let progress = tweened(0, { duration: 800, easing: cubicOut });
	let pathProgress = 0;
	let pathInterval: NodeJS.Timeout | undefined;

	// Particle generation (identical to original)
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

	// Start animations (identical to original handleGetStarted logic)
	progress.set(1);
	pathInterval = setInterval(() => {
		pathProgress = (pathProgress + 0.01) % 1;
	}, 20);

	setTimeout(() => {
		if (pathInterval) clearInterval(pathInterval);
		onComplete();
	}, 2500);
</script>

<div
	transition:fade={{ duration: 300, easing: cubicOut }}
	class="fixed inset-0 z-50 overflow-hidden bg-gradient-to-br from-blue-800 via-blue-600 to-blue-400"
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

	<!-- Main content -->
	<div class="absolute inset-0 flex flex-col items-center justify-center">
		<!-- Logo -->
		<div
			in:scale={{ duration: 800, delay: 200, easing: elasticOut, start: 0.5 }}
			class="mb-6 scale-150"
		>
			<SmpLogo fillColor="white" />
		</div>

		<!-- Animated text -->
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

		<!-- Progress bar -->
		<div class="mt-12 h-2 w-64 overflow-hidden rounded-full bg-white/20">
			<div
				class="h-full rounded-full bg-white"
				style="width: {$progress * 100}%; transition: width 2s cubic-bezier(0.34, 1.56, 0.64, 1);"
			></div>
		</div>

		<!-- Animated path -->
		<svg width="300" height="80" viewBox="0 0 300 80" class="mt-8 opacity-70">
			<defs>
				<linearGradient id="gradient" x1="0%" y1="0%" x2="100%" y2="0%">
					<stop offset="0%" stop-color="#2563EB" />
					<stop offset="100%" stop-color="#38BDF8" />
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
		class="pointer-events-none absolute inset-0"
		style="background: radial-gradient(circle, transparent 30%, rgba(0,0,0,0.2) 100%);"
	></div>
</div>

<style>
	@keyframes bounce {
		0% {
			transform: translateY(0);
		}
		100% {
			transform: translateY(-10px);
		}
	}
</style>
