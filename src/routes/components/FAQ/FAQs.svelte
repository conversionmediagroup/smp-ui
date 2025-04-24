<script lang="ts">
	import { onMount, tick } from 'svelte';
	import { fade, fly, scale } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
	import * as Accordion from '$lib/shadcn/accordion/index';
	import { MessageSquareMore, Search, ChevronRight, Filter, X } from 'lucide-svelte';

	// Animation and interaction state
	let visible = false;
	let searchQuery = '';
	let activeCategory = 'all';
	let isSearchFocused = false;

	// Categories for filtering
	const categories = [
		{ id: 'all', name: 'All Questions' },
		{ id: 'matching', name: 'Matching Process' },
		{ id: 'schools', name: 'Schools & Programs' },
		{ id: 'financial', name: 'Financial Aid' },
		{ id: 'application', name: 'Applications' }
	];

	// FAQ items with categories
	const allFaqItems = [
		{
			question: 'How does School Match Pro find the right schools for me?',
			answer:
				'School Match Pro uses a sophisticated algorithm that considers your academic background, career goals, location preferences, and budget to match you with accredited institutions that best fit your unique needs. We analyze thousands of data points to ensure you receive personalized recommendations.',
			category: 'matching'
		},
		{
			question: 'Is School Match Pro completely free to use?',
			answer:
				'Yes, our basic matching service is completely free for students. We partner with accredited colleges and universities who pay us when students find their perfect match. We may offer premium features in the future, but our core matching service will always remain free.',
			category: 'financial'
		},
		{
			question: 'How accurate are the school matches I receive?',
			answer:
				'Our matching algorithm has a 92% satisfaction rate among students who enrolled in their recommended schools. We continuously refine our system based on student feedback and outcomes to improve match accuracy.',
			category: 'matching'
		},
		{
			question: 'Can I get help with financial aid and scholarships?',
			answer:
				'We provide information about financial aid opportunities at each matched school, including scholarships, grants, and work-study programs. Our resources section also offers guides on completing FAFSA and other financial aid applications.',
			category: 'financial'
		},
		{
			question: 'Do you only match with four-year universities?',
			answer:
				'No, we match students with a wide range of accredited institutions including community colleges, technical schools, four-year universities, and online programs. Our goal is to find the right educational path for your specific goals, whatever form that may take.',
			category: 'schools'
		},
		{
			question: 'How long does the matching process take?',
			answer:
				"After completing your profile, you'll receive initial matches within minutes. Our system continues to refine your matches over time as you interact with recommendations. For personalized guidance, our education counselors can provide additional insights within 48 hours.",
			category: 'matching'
		},
		{
			question: "Can I use School Match Pro if I'm an international student?",
			answer:
				'Yes! We support international students looking to study in the United States. Our platform includes filters for schools that offer strong international student programs, visa support, and ESL resources.',
			category: 'schools'
		},
		{
			question: 'What information do I need to provide to get started?',
			answer:
				'To get basic matches, we need information about your academic interests, preferred locations, budget range, and career goals. The more details you provide in your profile, the more accurate your matches will be.',
			category: 'matching'
		},
		{
			question: 'Can School Match Pro help with the application process?',
			answer:
				'Yes, we offer application guidance for each matched school, including application requirements, deadlines, and tips for strengthening your application. Premium users can access personalized application review services and essay feedback.',
			category: 'application'
		},
		{
			question: 'How do you protect my personal information?',
			answer:
				'We take data privacy seriously. Your information is encrypted and securely stored. We only share your contact details with schools you explicitly express interest in. You can review our comprehensive privacy policy for more details.',
			category: 'application'
		}
	];

	// Filtered FAQ items based on search and category
	$: filteredFaqItems = allFaqItems.filter((item) => {
		const matchesSearch =
			searchQuery === '' ||
			item.question.toLowerCase().includes(searchQuery.toLowerCase()) ||
			item.answer.toLowerCase().includes(searchQuery.toLowerCase());

		const matchesCategory = activeCategory === 'all' || item.category === activeCategory;

		return matchesSearch && matchesCategory;
	});

	// Set category filter
	function setCategory(categoryId) {
		activeCategory = categoryId;
	}

	// Clear search
	function clearSearch() {
		searchQuery = '';
	}

	onMount(() => {
		visible = true;
	});
</script>

<section class="relative mx-auto max-w-5xl overflow-hidden px-6 py-24" id="faq">
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

	<!-- Header -->
	{#if visible}
		<div in:fade={{ duration: 800 }} class="mb-16 text-center">
			<div
				class="mx-auto mb-4 inline-flex items-center justify-center rounded-full bg-blue-100 px-4 py-2 text-blue-700"
			>
				<MessageSquareMore size={18} class="mr-2" />
				<span class="text-sm font-semibold">Frequently Asked Questions</span>
			</div>
			<h2 class="mb-4 text-4xl font-bold tracking-tight text-gray-900 md:text-5xl">
				Got Questions? <span class="text-blue-600">We've Got Answers</span>
			</h2>
			<p class="mx-auto max-w-2xl text-lg text-gray-600">
				Everything you need to know about finding your perfect school match. Can't find what you're
				looking for?
				<a href="#contact" class="font-medium text-blue-600 hover:text-blue-700 hover:underline"
					>Contact our support team</a
				>.
			</p>
		</div>
	{/if}

	<!-- FAQ Accordion -->
	{#if visible}
		<div in:fly={{ y: 20, duration: 800, delay: 200 }}>
			{#if filteredFaqItems.length > 0}
				<div class="grid gap-4">
					<Accordion.Root type="multiple" class="w-full max-w-full">
						{#each filteredFaqItems as item, index}
							<Accordion.Item
								value={`item-${index}`}
								class="group mb-4 overflow-hidden rounded-2xl border border-gray-200 bg-white shadow-sm transition-all duration-300 hover:border-blue-200 hover:shadow-md"
							>
								<Accordion.Trigger
									class="group flex w-full items-center justify-between p-3 text-left text-sm text-gray-800"
								>
									<span class="transition-colors duration-200">{item.question}</span>
									<!-- <div
										class="flex h-8 w-8 items-center justify-center rounded-full bg-gray-100 text-gray-500 transition-all duration-300 group-hover:bg-blue-100 group-hover:text-blue-600 group-data-[state=open]:rotate-90 group-data-[state=open]:bg-blue-100 group-data-[state=open]:text-blue-600"
									>
										<ChevronRight size={18} class="transition-transform duration-300" />
									</div> -->
								</Accordion.Trigger>
								<Accordion.Content
									class="data-[state=closed]:animate-accordion-up data-[state=open]:animate-accordion-down max-w-[750px] overflow-hidden"
								>
									<div class="w-full px-8 pt-0 pb-8">
										<div class="mb-6 h-px w-full bg-gray-100"></div>
										<p class="leading-relaxed text-gray-600">{item.answer}</p>

										<!-- Category tag -->
										<div class="mt-6 flex items-center">
											<span
												class="rounded-full bg-blue-50 px-3 py-1 text-xs font-medium text-blue-700"
											>
												{categories.find((cat) => cat.id === item.category)?.name || 'General'}
											</span>
										</div>
									</div>
								</Accordion.Content>
							</Accordion.Item>
						{/each}
					</Accordion.Root>
				</div>
			{:else}
				<div
					in:scale={{ duration: 300 }}
					class="rounded-2xl border border-gray-200 bg-gray-50 py-16 text-center"
				>
					<div
						class="mx-auto mb-4 flex h-16 w-16 items-center justify-center rounded-full bg-blue-100"
					>
						<Search size={24} class="text-blue-600" />
					</div>
					<h3 class="mb-2 text-xl font-semibold text-gray-800">No questions found</h3>
					<p class="mb-6 text-gray-500">We couldn't find any questions matching "{searchQuery}"</p>
					<button
						on:click={() => {
							searchQuery = '';
							activeCategory = 'all';
						}}
						class="inline-flex items-center rounded-full bg-blue-600 px-6 py-3 text-sm font-medium text-white shadow-sm transition-colors hover:bg-blue-700"
					>
						Clear all filters
					</button>
				</div>
			{/if}
		</div>
	{/if}

	<!-- Contact CTA -->
	{#if visible}
		<div in:fly={{ y: 20, duration: 800, delay: 400 }} class="mt-16 text-center">
			<p class="mb-4 text-gray-600">Still have questions?</p>
			<a
				href="#contact"
				class="inline-flex items-center rounded-full bg-blue-600 px-6 py-3 text-sm font-medium text-white shadow-sm transition-colors hover:bg-blue-700"
			>
				Contact our support team
				<ChevronRight size={16} class="ml-1" />
			</a>
		</div>
	{/if}
</section>

<style>
	/* Optional: Add these animations if not already in your global styles */
	@keyframes accordion-down {
		from {
			height: 0;
		}
		to {
			height: var(--radix-accordion-content-height);
		}
	}

	@keyframes accordion-up {
		from {
			height: var(--radix-accordion-content-height);
		}
		to {
			height: 0;
		}
	}

	:global(.animate-accordion-down) {
		animation: accordion-down 0.2s ease-out;
	}

	:global(.animate-accordion-up) {
		animation: accordion-up 0.2s ease-out;
	}

	/* Added styles for accordion content width */
	:global(.data-[state='open']:animate-accordion-down) {
		width: 100%;
		max-width: 100%;
	}

	:global(.data-[state='closed']:animate-accordion-up) {
		width: 100%;
		max-width: 100%;
	}
</style>
