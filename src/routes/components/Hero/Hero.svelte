<script lang="ts">
	import HeroImg from '$lib/assets/test.jpg';
	import { onMount } from 'svelte';

	let isLoaded = false;

	onMount(() => {
		setTimeout(() => {
			isLoaded = true;
		}, 300);
	});
</script>

<section
	class="relative overflow-hidden bg-gradient-to-br from-blue-50 to-indigo-50 py-12 lg:min-h-[700px] lg:py-20"
>
	<!-- Decorative elements -->
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
					<!-- Original button restored -->
					<button
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
					<div
						class="absolute inset-0 z-10 bg-gradient-to-tr from-blue-500/20 to-indigo-500/20 mix-blend-overlay"
					></div>
					<img
						src={HeroImg || '/placeholder.svg'}
						alt="School Match Pro"
						class="h-auto w-full object-cover transition-transform duration-700 hover:scale-105 lg:h-[500px]"
					/>

					<!-- Floating elements -->
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
</style>
