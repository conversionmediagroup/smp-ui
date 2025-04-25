<script lang="ts">
	import HeroImg from '$lib/assets/test.jpg';
	import HeroImg2 from '$lib/assets/smp2.png';
	import HeroImg3 from '$lib/assets/smp3.png';
	import HeroImg4 from '$lib/assets/smp4.png';
	import HeroImg5 from '$lib/assets/smp5.png';
	import { onMount, onDestroy } from 'svelte';
	import { fade } from 'svelte/transition';

	// Array of all images
	const images = [HeroImg, HeroImg2, HeroImg3, HeroImg4, HeroImg5];

	// Current image index
	let currentIndex = 0;

	// Control visibility for fade effect
	let visible = true;

	// Interval ID for cleanup
	let intervalId: ReturnType<typeof setInterval>;

	// Transition duration in ms
	const transitionDuration = 1000;

	// Time each image is displayed in ms (including transition time)
	const displayDuration = 5000;

	// Function to advance to next image with fade effect
	const nextImage = () => {
		visible = false; // Trigger fade out

		// Wait for fade out to complete before changing image
		setTimeout(() => {
			currentIndex = (currentIndex + 1) % images.length;
			visible = true; // Trigger fade in
		}, transitionDuration);
	};

	onMount(() => {
		// Start the slideshow
		intervalId = setInterval(nextImage, displayDuration);

		return () => {
			clearInterval(intervalId);
		};
	});

	onDestroy(() => {
		clearInterval(intervalId);
	});
</script>

<div class="relative h-full w-full overflow-hidden rounded-2xl">
	<div
		class="absolute inset-0 z-10 bg-gradient-to-tr from-blue-500/20 to-indigo-500/20 mix-blend-overlay"
	></div>

	{#if visible}
		<img
			src={images[currentIndex] || '/placeholder.svg'}
			alt={`School Match Pro Image ${currentIndex + 1}`}
			class="h-auto w-full object-cover transition-transform duration-700 hover:scale-105 lg:h-[500px]"
			transition:fade={{ duration: transitionDuration }}
		/>
	{/if}

	<div class="absolute right-4 bottom-4 z-20 flex gap-1.5">
		{#each images as _, i}
			<div
				class="h-2 w-2 rounded-full transition-all duration-300 {i === currentIndex
					? 'scale-125 bg-white'
					: 'bg-white/50'}"
			></div>
		{/each}
	</div>

	<div
		class="absolute top-4 right-4 z-20 rotate-3 transform rounded-lg bg-white/90 p-2 shadow-lg backdrop-blur-sm"
	>
		<div class="flex items-center gap-2">
			<div class="h-3 w-3 rounded-full bg-blue-500"></div>
			<span class="text-xs font-medium text-slate-800">Top Rated</span>
		</div>
	</div>

	<div
		class="absolute bottom-4 left-4 z-20 -rotate-2 transform rounded-lg bg-white/90 p-2 shadow-lg backdrop-blur-sm"
	>
		<div class="flex items-center gap-2">
			<div class="h-3 w-3 rounded-full bg-indigo-500"></div>
			<span class="text-xs font-medium text-slate-800">1000+ Schools</span>
		</div>
	</div>
</div>
