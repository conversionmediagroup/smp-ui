<script lang="ts">
	import { onMount } from 'svelte';
	import logo from '../../../../static/assets/smp-logo.svg';
	import MobileMenuBtn from './MobileMenuBtn.svelte';
	import { cn } from '$lib/ultils';
	import SmpLogo from './SMPLogo.svelte';

	let mobileMenuOpen = false;

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
		'sticky top-0 z-20 border-b-2 border-white bg-blue-600 p-2 px-4 shadow-sm duration-300 lg:px-8'
	)}
>
	<div class="flex justify-between">
		<a
			href="/"
			class="focus-visible:ring-opacity-75 flex items-center transition-opacity hover:opacity-90 focus:outline-none focus-visible:ring-2 focus-visible:ring-white"
			aria-label="Go to homepage"
		>
			<div class="flex items-center">
				<img
					src={logo || '/placeholder.svg'}
					alt="School Match Pro logo"
					class="scale-70 md:hidden"
				/>
				<SmpLogo fillColor="white" />
			</div>
		</a>

		<MobileMenuBtn bind:menuButton />
	</div>
</header>
