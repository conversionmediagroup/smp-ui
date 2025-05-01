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

<footer class="relative mt-auto overflow-hidden bg-blue-600 py-8 text-white shadow-md">
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

	<div class="container mx-auto px-4">
		<div class="grid grid-cols-1 gap-x-8 gap-y-6 md:grid-cols-3">
			{#if visible}
				<!-- Company Info -->
				<div in:fly={{ y: 20, duration: 800, delay: 300 }} class="relative z-10">
					<div class="mb-3 max-w-[160px]">
						<SmpLogo fillColor="white" />
					</div>
					<p class="text-sm leading-relaxed text-white/90">
						Connecting prospective students with top accredited colleges and universities across the
						United States.
					</p>
				</div>
			{/if}

			<!-- Quick Links -->
			{#if visible}
				<div in:fly={{ y: 20, duration: 800, delay: 400 }} class="relative z-10">
					<h3 class="mb-3 text-base font-bold text-white">Quick Links</h3>
					<ul class="grid grid-cols-1 gap-2">
						{#each ['Home', 'About Us', 'Contact'] as item}
							<li>
								<a
									href="#"
									class="group flex items-center text-sm text-white/90 transition-all hover:text-white"
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
					<h3 class="mb-3 text-base font-bold text-white">Contact Us</h3>
					<ul class="grid gap-2 text-sm text-white/90">
						<li>
							<a
								href="mailto:info@schoolmatchpro.com"
								class="group flex items-center transition-all hover:text-white"
							>
								<Mail size={14} class="mr-2 text-white transition-all group-hover:scale-110" />
								<span class="group-hover:underline">info@schoolmatchpro.com</span>
							</a>
						</li>
						<li>
							<a
								href="tel:+18005551234"
								class="group flex items-center transition-all hover:text-white"
							>
								<Phone size={14} class="mr-2 text-white transition-all group-hover:scale-110" />
								<span class="group-hover:underline">(800) 555-1234</span>
							</a>
						</li>
						<li class="flex">
							<MapPin size={14} class="mt-0.5 mr-2 shrink-0 text-white" />
							<span>
								123 Education Lane, Suite 100
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
			<div in:fly={{ y: 20, duration: 800, delay: 600 }} class="relative z-10 mt-6 pt-4">
				<div
					class="absolute top-0 right-0 left-0 h-px bg-gradient-to-r from-transparent via-blue-200 to-transparent"
				></div>

				<div
					class="flex flex-col items-center justify-between space-y-2 text-center md:flex-row md:space-y-0"
				>
					<p class="text-xs text-white/80">
						© {new Date().getFullYear()} SchoolMatchPro. All rights reserved.
					</p>
					<div class="flex flex-wrap justify-center gap-x-6 gap-y-1">
						{#each ['Privacy Policy', 'Terms of Service'] as item}
							<a
								href="#"
								class="text-xs text-white/80 transition-all hover:text-white hover:underline"
							>
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
			class="fixed right-6 bottom-6 z-50 flex h-10 w-10 items-center justify-center rounded-full bg-gradient-to-br from-blue-500 to-blue-400 text-white shadow-lg transition-all hover:-translate-y-1 hover:shadow-lg hover:shadow-blue-500/20"
			aria-label="Back to top"
		>
			<ChevronUp size={18} />
		</button>
	{/if}
</footer>
