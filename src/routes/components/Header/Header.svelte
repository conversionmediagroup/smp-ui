<script lang="ts">
	import { onMount } from 'svelte';
	import { cn } from '$lib/ultils';

	import SmpLogo from './SMPLogo.svelte';
	import logo from '../../../../static/assets/smp-logo.svg';

	let isScrolled = false;

	function handleScroll() {
		isScrolled = window.scrollY > 50;
	}

	onMount(() => {
		window.addEventListener('scroll', handleScroll);
		return () => window.removeEventListener('scroll', handleScroll);
	});

	const navLinks = [
		{ name: 'Why Choose Us', href: '#why-choose-us' },
		{ name: 'Partners', href: '#school-partners' },
		{ name: 'FAQ', href: '#faq' }
	];

	function scrollToSection(event, sectionId) {
		event.preventDefault();
		const section = document.querySelector(sectionId);
		if (section) {
			section.scrollIntoView({
				behavior: 'smooth'
			});
		}
	}
</script>

<header
	class={cn(
		'sticky top-0 z-20 border-b-2 border-white bg-blue-600 p-2 px-4 shadow-sm duration-300 lg:px-8',
		isScrolled ? 'py-2' : 'py-3'
	)}
>
	<div class="flex flex-wrap items-center justify-between gap-4">
		<a
			href="/"
			class="focus-visible:ring-opacity-75 flex items-center transition-opacity hover:opacity-90 focus:outline-none focus-visible:ring-2 focus-visible:ring-white"
			aria-label="Go to homepage"
		>
			<div class="flex items-center">
				<img src={logo} alt="School Match Pro logo" class="scale-70 sm:hidden" />
				<SmpLogo fillColor="white" />
			</div>
		</a>

		<nav>
			<ul class="flex items-center space-x-4 sm:space-x-8">
				{#each navLinks as link}
					<li>
						<a
							href={link.href}
							class="relative text-sm font-medium text-white transition-colors duration-200 after:absolute after:bottom-0 after:left-0 after:h-0.5 after:w-0 after:bg-white after:transition-all hover:text-sky-100 hover:after:w-full sm:text-base"
							on:click={(e) => scrollToSection(e, link.href)}
						>
							{link.name}
						</a>
					</li>
				{/each}
			</ul>
		</nav>
	</div>
</header>

<style>
	@keyframes fadeIn {
		from {
			opacity: 0;
			transform: translateY(-10px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	nav ul li {
		animation: fadeIn 0.5s ease forwards;
		animation-delay: calc(0.1s * var(--index, 0));
		opacity: 0;
	}

	nav ul li:nth-child(1) {
		--index: 1;
	}
	nav ul li:nth-child(2) {
		--index: 2;
	}
	nav ul li:nth-child(3) {
		--index: 3;
	}
	nav ul li:nth-child(4) {
		--index: 4;
	}
</style>
