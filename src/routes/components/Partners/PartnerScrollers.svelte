<script lang="ts">
	import { onMount } from 'svelte';
	import { fade, fly } from 'svelte/transition';
	import { Building, Award } from 'lucide-svelte';

	let scrollers1: HTMLDivElement;
	let scrollerInner1: HTMLUListElement;
	let visible = false;

	import p1 from '$lib/assets/partners/partner1.png';
	import p2 from '$lib/assets/partners/partner2.png';
	import p3 from '$lib/assets/partners/partner3.png';
	import p4 from '$lib/assets/partners/partner4.png';
	import p5 from '$lib/assets/partners/partner5.png';
	import p6 from '$lib/assets/partners/partner6.png';
	import p7 from '$lib/assets/partners/partner7.png';
	import p8 from '$lib/assets/partners/partner8.png';

	const duplicateItemsForScroller = (
		scrollers: HTMLDivElement,
		scrollerInner: HTMLUListElement
	) => {
		const scroller = scrollers.childNodes;
		const duplicatedItems = [];
		scroller.forEach((item) => {
			if (item instanceof HTMLElement) {
				item.setAttribute('data-animated', 'true');
			}
			const scrollerContent = Array.from(scrollerInner.children);
			scrollerContent.forEach((item) => {
				if (item instanceof HTMLElement) {
					const duplicatedItem = item.cloneNode(true) as HTMLElement;
					duplicatedItem.setAttribute('aria-hidden', 'true');
					scrollerInner.appendChild(duplicatedItem);
					duplicatedItems.push(duplicatedItem);
				}
			});
		});
	};

	onMount(() => {
		duplicateItemsForScroller(scrollers1, scrollerInner1);
		setTimeout(() => {
			visible = true;
		}, 300);
	});

	let valuedPartnerLogos = [p1, p2, p3, p4, p5, p6, p7, p8];
</script>

<section class="relative py-20">
	<!-- Decorative elements -->
	<div
		class="absolute top-0 right-0 -z-10 h-96 w-96 rounded-full bg-blue-50 opacity-70 blur-[100px]"
	></div>
	<div
		class="absolute bottom-0 left-0 -z-10 h-96 w-96 rounded-full bg-indigo-50 opacity-70 blur-[100px]"
	></div>

	<!-- Subtle pattern overlay -->
	<div
		class="absolute inset-0 -z-10 bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAiIGhlaWdodD0iNjAiIHZpZXdCb3g9IjAgMCA2MCA2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZyBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxnIGZpbGw9IiMwMDAiIGZpbGwtb3BhY2l0eT0iMSI+PHBhdGggZD0iTTM2IDM0YzAtMi4yMS0xLjc5LTQtNC00cy00IDEuNzktNCA0IDEuNzkgNCA0IDQgNC0xLjc5IDQtNHpNMjQgMzBjMC0yLjIxLTEuNzktNC00LTRzLTQgMS43OS00IDQgMi43OSA0IDQgNCA0LTEuNzkgNC00em0yNCAwYzAtMi4yMS0xLjc5LTQtNC00cy00IDEuNzktNCA0IDEuNzkgNCA0IDQgNC0xLjc5IDQtNHoiLz48L2c+PC9nPjwvc3ZnPg==')] opacity-[0.03]"
	></div>

	<div class="container mx-auto px-4">
		<!-- Header styled similar to FAQ section -->
		{#if visible}
			<div in:fade={{ duration: 800 }} class="mb-12 text-center">
				<div
					class="mx-auto mb-4 inline-flex items-center justify-center rounded-full bg-blue-100 px-4 py-2 text-blue-700"
				>
					<Building size={18} class="mr-2" />
					<span class="text-sm font-semibold">Trusted Institutions</span>
				</div>
				<h2 class="mb-4 text-4xl font-bold tracking-tight text-gray-900 md:text-5xl">
					Our <span class="text-blue-600">School Partners</span>
				</h2>
				<p class="mx-auto max-w-2xl text-lg text-gray-600">
					We collaborate with top accredited institutions nationwide to help you find your perfect
					educational match.
				</p>
			</div>
		{/if}

		<!-- Logo scroller -->
		{#if visible}
			<div
				in:fly={{ y: 20, duration: 800, delay: 200 }}
				class="lg-desktop:justify-between lg-desktop:pt-6 flex flex-col"
			>
				<div
					class="maskEffect relative mx-auto max-w-[90vw] self-center overflow-hidden rounded-xl border border-gray-100 bg-white py-8 whitespace-nowrap shadow-sm lg:max-w-[1200px]"
					bind:this={scrollers1}
				>
					<!-- Gradient edges for smooth fade -->
					<div
						class="absolute inset-y-0 left-0 z-10 w-16 bg-gradient-to-r from-white to-transparent"
					></div>
					<div
						class="absolute inset-y-0 right-0 z-10 w-16 bg-gradient-to-l from-white to-transparent"
					></div>

					<ul
						class="motion-safe:animate-valued-partners-infinite-scroll lg-desktop:motion-safe:animate-valued-partners-infinite-scroll-lg-desktop flex w-max flex-wrap items-center gap-x-12 py-4 lg:gap-x-20"
						bind:this={scrollerInner1}
					>
						{#each valuedPartnerLogos as item, i}
							<li class="flex items-center justify-center">
								<img
									src={item || '/placeholder.svg'}
									class="lg-desktop:scale-110 max-h-[80px] max-w-[140px] grayscale transition-all duration-300 hover:scale-110 hover:grayscale-0"
									alt="Partner logo"
								/>
							</li>
						{/each}
					</ul>
				</div>

				<!-- Partner badge -->
				{#if visible}
					<div in:fly={{ y: 20, duration: 800, delay: 400 }} class="mt-8 flex justify-center">
						<div
							class="inline-flex items-center rounded-full border border-blue-100 bg-blue-50 px-4 py-2"
						>
							<Award size={16} class="mr-2 text-blue-600" />
							<span class="text-sm font-medium text-blue-700"
								>Partnered with {valuedPartnerLogos.length}+ accredited institutions</span
							>
						</div>
					</div>
				{/if}
			</div>
		{/if}
	</div>
</section>

<style lang="postcss">
	.maskEffect {
		mask: linear-gradient(90deg, transparent, white 20%, white 80%, transparent);
	}

	/* Animation for the logo scroller */
	@keyframes scroll {
		from {
			transform: translateX(0);
		}
		to {
			transform: translateX(calc(-50% - 0.5rem));
		}
	}

	:global(.animate-valued-partners-infinite-scroll) {
		animation: scroll 30s linear infinite;
	}

	:global(.animate-valued-partners-infinite-scroll-lg-desktop) {
		animation: scroll 40s linear infinite;
	}

	@media (prefers-reduced-motion: reduce) {
		:global(.animate-valued-partners-infinite-scroll),
		:global(.animate-valued-partners-infinite-scroll-lg-desktop) {
			animation: none;
		}
	}
</style>
