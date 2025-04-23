<script lang="ts">
	import { fade } from 'svelte/transition';
	import { onMount } from 'svelte';
	import logo from '../../../../static/assets/smp-logo.svg';
	import MobileMenuBtn from './MobileMenuBtn.svelte';
	import { cn } from '$lib/ultils';

	const navItems = [
		{ name: 'Home', href: '#' },
		{ name: 'Programs', href: '#' },
		{ name: 'About Us', href: '#' },
		{ name: 'Contact', href: '#' }
	];

	let mobileMenuOpen = false;
	let isScrolled = false;
	let menuButton: HTMLDivElement;

	function toggleMobileMenu() {
		mobileMenuOpen = !mobileMenuOpen;
	}

	function handleScroll() {
		isScrolled = window.scrollY > 50; // Trigger effect after 50px scroll
	}

	onMount(() => {
		window.addEventListener('scroll', handleScroll);
		return () => window.removeEventListener('scroll', handleScroll);
	});
</script>

<header
	class={cn(
		'sticky top-0 z-10 m-2 rounded-full px-2 shadow-sm transition-colors duration-300',
		isScrolled ? 'bg-blue-500/80 backdrop-blur-md' : 'bg-blue-500'
	)}
>
	<div class="flex justify-between p-1">
		<div class="flex items-center">
			<img src={logo} alt="School Match Pro logo" class="scale-70" />
		</div>

		<!-- <MobileMenuBtn bind:menuButton /> -->

		<!-- <nav class="hidden space-x-8 md:flex">
			{#each navItems as item}
				<a href={item.href} class="font-medium text-gray-600 hover:text-blue-600">
					{item.name}
				</a>
			{/each}
		</nav> -->
	</div>

	<!-- {#if mobileMenuOpen}
		<nav class="mt-4 border-t pt-4 pb-2 md:hidden" transition:fade={{ duration: 200 }}>
			<ul class="space-y-3">
				{#each navItems as item}
					<li>
						<a href={item.href} class="block font-medium text-gray-600 hover:text-blue-600">
							{item.name}
						</a>
					</li>
				{/each}
			</ul>
		</nav>
	{/if} -->
</header>
