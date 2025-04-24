<script lang="ts">
	import { onMount } from 'svelte';
	import { fade, fly, scale } from 'svelte/transition';
	import { Mail, Phone, MapPin, Facebook, Twitter, Linkedin, ChevronUp, Send } from 'lucide-svelte';
	import SmpLogo from '../Header/SMPLogo.svelte';

	// Animation state
	let visible = false;
	let showBackToTop = false;

	// Form state
	let email = '';
	let subscribed = false;

	// Handle newsletter subscription
	function handleSubscribe() {
		if (email && email.includes('@')) {
			subscribed = true;
			setTimeout(() => {
				email = '';
				subscribed = false;
			}, 3000);
		}
	}

	// Handle scroll for back-to-top button
	function scrollToTop() {
		window.scrollTo({ top: 0, behavior: 'smooth' });
	}

	// Floating particles
	let particles = Array(8)
		.fill(0)
		.map((_, i) => ({
			id: i,
			x: Math.random() * 100,
			y: Math.random() * 100,
			size: 2 + Math.random() * 4,
			speed: 0.2 + Math.random() * 0.3,
			opacity: 0.3 + Math.random() * 0.4
		}));

	onMount(() => {
		visible = true;

		// Show back to top button on scroll
		const handleScroll = () => {
			showBackToTop = window.scrollY > 300;
		};

		window.addEventListener('scroll', handleScroll);

		// Animation loop for particles
		let frame;
		const animateParticles = () => {
			particles = particles.map((p) => ({
				...p,
				y: (p.y + p.speed) % 100
			}));
			frame = requestAnimationFrame(animateParticles);
		};

		frame = requestAnimationFrame(animateParticles);

		return () => {
			window.removeEventListener('scroll', handleScroll);
			cancelAnimationFrame(frame);
		};
	});
</script>

<footer class="relative mt-auto overflow-hidden bg-blue-600 pt-16 pb-8 text-white shadow-sm">
	<!-- Decorative elements with Svelte transitions -->
	{#if visible}
		<div
			in:fade={{ delay: 200, duration: 1000 }}
			class="absolute -top-24 -right-24 h-48 w-48 rounded-full bg-blue-400/10 blur-3xl"
		></div>
		<div
			in:fade={{ delay: 400, duration: 1000 }}
			class="absolute top-1/2 -left-24 h-48 w-48 rounded-full bg-sky-300/10 blur-3xl"
		></div>
		<div
			in:fade={{ delay: 600, duration: 1000 }}
			class="absolute right-1/4 bottom-0 h-64 w-64 rounded-full bg-blue-300/10 blur-3xl"
		></div>
	{/if}

	<!-- Accent line -->
	<div
		in:scale={{ duration: 1000, start: 0.8, opacity: 0 }}
		class="absolute top-0 right-0 left-0 h-1 bg-gradient-to-r from-sky-300 via-blue-400 to-sky-300"
	></div>

	<div class="container mx-auto px-4">
		<div class="grid grid-cols-1 gap-12 md:grid-cols-3">
			<!-- Company Info -->
			{#if visible}
				<div in:fly={{ y: 20, duration: 800, delay: 300 }} class="relative z-10">
					<div class="mb-6">
						<SmpLogo fillColor="white" />
					</div>
					<p class="mb-6 text-white">
						Connecting prospective students with top accredited colleges and universities across the
						United States.
					</p>
					<!-- <div class="flex space-x-4">
						<a
							href="#"
							class="flex h-10 w-10 items-center justify-center rounded-full bg-gradient-to-br from-blue-400 to-blue-500 text-white transition-all hover:-translate-y-1 hover:shadow-lg hover:shadow-blue-200"
						>
							<Facebook size={20} />
							<span class="sr-only">Facebook</span>
						</a>
						<a
							href="#"
							class="flex h-10 w-10 items-center justify-center rounded-full bg-gradient-to-br from-blue-400 to-blue-500 text-white transition-all hover:-translate-y-1 hover:shadow-lg hover:shadow-blue-200"
						>
							<Twitter size={20} />
							<span class="sr-only">Twitter</span>
						</a>
						<a
							href="#"
							class="flex h-10 w-10 items-center justify-center rounded-full bg-gradient-to-br from-blue-400 to-blue-500 text-white transition-all hover:-translate-y-1 hover:shadow-lg hover:shadow-blue-200"
						>
							<Linkedin size={20} />
							<span class="sr-only">LinkedIn</span>
						</a>
					</div> -->
				</div>
			{/if}

			<!-- Quick Links -->
			{#if visible}
				<div in:fly={{ y: 20, duration: 800, delay: 400 }} class="relative z-10">
					<h3 class="mb-6 text-xl font-bold text-white">Quick Links</h3>
					<ul class="space-y-3">
						{#each ['Home', 'About Us', 'Programs', 'Partner Schools', 'Contact'] as item}
							<li>
								<a
									href="#"
									class="group flex items-center text-white/90 transition-all hover:text-white"
								>
									<span class="mr-2 h-1 w-0 rounded-full bg-blue-400 transition-all group-hover:w-3"
									></span>
									{item}
								</a>
							</li>
						{/each}
					</ul>
				</div>
			{/if}

			<!-- Contact -->
			{#if visible}
				<div in:fly={{ y: 20, duration: 800, delay: 500 }} class="relative z-10">
					<h3 class="mb-6 text-xl font-bold text-white">Contact Us</h3>
					<ul class="space-y-4 text-white">
						<li>
							<a
								href="mailto:info@schoolmatchpro.com"
								class="group flex items-center transition-all"
							>
								<Mail size={18} class="mr-3 text-white transition-all group-hover:scale-110" />
								<span class="group-hover:underline">info@schoolmatchpro.com</span>
							</a>
						</li>
						<li>
							<a href="tel:+18005551234" class="group flex items-center transition-all">
								<Phone size={18} class="mr-3 text-white transition-all group-hover:scale-110" />
								<span class="group-hover:underline">(800) 555-1234</span>
							</a>
						</li>
						<li class="flex">
							<MapPin size={18} class="mt-1 mr-3 shrink-0 text-white" />
							<span>
								123 Education Lane
								<br />
								Suite 100
								<br />
								San Francisco, CA 94107
							</span>
						</li>
					</ul>
				</div>
			{/if}
		</div>

		<!-- Bottom section with copyright -->
		{#if visible}
			<div in:fly={{ y: 20, duration: 800, delay: 600 }} class="relative z-10 mt-12 pt-8">
				<div
					class="absolute top-0 right-0 left-0 h-px bg-gradient-to-r from-transparent via-blue-200 to-transparent"
				></div>

				<div
					class="flex flex-col items-center justify-between space-y-4 text-center md:flex-row md:space-y-0"
				>
					<p class="text-sm text-white">
						© {new Date().getFullYear()} SchoolMatchPro. All rights reserved.
					</p>
					<div class="flex flex-wrap justify-center gap-x-6 gap-y-2">
						{#each ['Privacy Policy', 'Terms of Service'] as item}
							<a href="#" class="text-sm text-white transition-all hover:underline">
								{item}
							</a>
						{/each}
					</div>
				</div>
			</div>
		{/if}
	</div>

	<!-- Back to top button -->
	{#if showBackToTop}
		<button
			on:click={scrollToTop}
			in:scale
			out:fade
			class="fixed right-8 bottom-8 z-50 flex h-12 w-12 items-center justify-center rounded-full bg-gradient-to-br from-blue-400 to-blue-500 text-white shadow-lg transition-all hover:shadow-blue-200/50"
			aria-label="Back to top"
		>
			<ChevronUp size={24} />
		</button>
	{/if}
</footer>
