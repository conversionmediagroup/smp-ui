<script lang="ts">
	import { onMount } from 'svelte';
	import { fade, fly, scale } from 'svelte/transition';
	import { Shield, Award, Badge, CheckCircle, ChevronRight, Star } from 'lucide-svelte';

	import p1 from '$lib/assets/partners/partner1.png';
	import p2 from '$lib/assets/partners/partner2.png';
	import p3 from '$lib/assets/partners/partner3.png';
	import p4 from '$lib/assets/partners/partner4.png';
	import p5 from '$lib/assets/partners/partner5.png';
	import p6 from '$lib/assets/partners/partner6.png';
	import p7 from '$lib/assets/partners/partner7.png';
	import p8 from '$lib/assets/partners/partner8.png';

	let visible = false;
	let activeTab = 0;

	const partners = [
		{
			name: 'Arizona State University',
			logo: p1
		},
		{
			name: 'Keiser University',
			logo: p2
		},
		{
			name: 'Northcentral University',
			logo: p3,
			description: 'Institutional quality assurance'
		},
		{
			name: 'Liberty University',
			logo: p4,
			description: 'National advocate for academic quality'
		},
		{
			name: 'Keller of DeVry University',
			logo: p5
		},
		{
			name: 'University of Maryland',
			logo: p6
		},
		{
			name: 'University of Massachusettes',
			logo: p7,
			description: 'Educational excellence advocate'
		},
		{
			name: 'University of Massachusettes Amherst',
			logo: p8
		}
	];

	const benefits = [
		{
			icon: Shield,
			title: 'Quality Assurance',
			description:
				'All partner schools meet rigorous academic standards to ensure you receive a high-quality education that prepares you for success.'
		},
		{
			icon: Award,
			title: 'Recognized Credentials',
			description:
				'Degrees from our accredited institutions are widely respected by employers and other educational institutions worldwide.'
		},
		{
			icon: Badge,
			title: 'Financial Aid Eligibility',
			description:
				'Students at accredited schools can access federal loans, grants, and scholarships to help fund their education journey.'
		},
		{
			icon: CheckCircle,
			title: 'Credit Transferability',
			description:
				'Credits earned at accredited institutions can be transferred between schools, giving you flexibility in your educational path.'
		}
	];

	const stats = [
		{ value: 1000, label: 'Accredited Schools', suffix: '+' },
		{ value: 50000, label: 'Students Matched', suffix: '+' },
		{ value: 98, label: 'Satisfaction Rate', suffix: '%' },
		{ value: 25, label: 'Years Experience', suffix: '+' }
	];

	let counters = stats.map((stat) => ({ current: 0, target: stat.value }));
	let counterStarted = false;

	function animateCounters() {
		if (counterStarted) return;
		counterStarted = true;

		stats.forEach((stat, i) => {
			const duration = 2000; // 2 seconds
			const steps = 60;
			const increment = stat.value / steps;
			let current = 0;
			const timer = setInterval(() => {
				current += increment;
				if (current >= stat.value) {
					current = stat.value;
					clearInterval(timer);
				}
				counters[i].current = Math.floor(current);
			}, duration / steps);
		});
	}

	onMount(() => {
		setTimeout(() => {
			visible = true;
		}, 300);

		const observer = new IntersectionObserver(
			(entries) => {
				if (entries[0].isIntersecting) {
					visible = true;
					animateCounters();
					observer.disconnect();
				}
			},
			{ threshold: 0.2 }
		);

		const section = document.getElementById('accredited-section');
		if (section) observer.observe(section);

		return () => {
			if (section) observer.disconnect();
		};
	});

	let partnerIndex = 0;
	let partnerInterval;

	onMount(() => {
		partnerInterval = setInterval(() => {
			partnerIndex = (partnerIndex + 1) % partners.length;
		}, 3000);

		return () => {
			clearInterval(partnerInterval);
		};
	});

	function setActiveTab(index) {
		activeTab = index;
	}
</script>

<section
	id="why-choose-us"
	class="relative overflow-hidden bg-gradient-to-b from-white to-blue-50 py-20"
>
	<div class="absolute top-0 left-0 h-64 w-64 rounded-full bg-blue-100/30 blur-3xl"></div>
	<div class="absolute right-0 bottom-0 h-80 w-80 rounded-full bg-indigo-100/30 blur-3xl"></div>

	<div class="absolute inset-0 opacity-5">
		<div
			class="h-full w-full bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAiIGhlaWdodD0iNjAiIHZpZXdCb3g9IjAgMCA2MCA2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZyBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxnIGZpbGw9IiMwMDAiIGZpbGwtb3BhY2l0eT0iLjAyIj48cGF0aCBkPSJNMzYgMzRjMC0yLjIxLTEuNzktNC00LTRzLTQgMS43OS00IDQgMS43OSA0IDQgNCA0LTEuNzkgNC00ek0yNCAzMGMwLTIuMjEtMS43OS00LTQtNHMtNCAxLjc5LTQgNCAyLjc5IDQgNCA0IDQtMS43OSA0LTR6bTI0IDBjMC0yLjIxLTEuNzktNC00LTRzLTQgMS43OS00IDQgMS43OSA0IDQgNCA0LTEuNzkgNC00eiIvPjwvZz48L2c+PC9zdmc+')]"
		></div>
	</div>

	<div class="relative z-10 container mx-auto px-4">
		{#if visible}
			<div class="mb-16 text-center">
				<div
					in:scale={{ duration: 600, start: 0.8 }}
					class="mx-auto mb-4 inline-flex items-center rounded-full bg-blue-100 px-4 py-1.5 text-sm font-medium text-blue-700"
				>
					<span class="mr-2 flex h-2 w-2 rounded-full bg-blue-500"></span>
					Trusted by thousands of students
				</div>

				<h2
					in:fly={{ y: -20, duration: 800, delay: 200 }}
					class="mb-4 bg-blue-600 bg-clip-text text-4xl font-bold tracking-tight text-transparent md:text-5xl"
				>
					Why Choose School Match Pro
				</h2>

				<p
					in:fly={{ y: 20, duration: 800, delay: 400 }}
					class="mx-auto max-w-2xl text-lg text-gray-600"
				>
					Our network includes only institutions that meet the highest standards of educational
					excellence, ensuring your future is in good hands.
				</p>
			</div>
		{/if}

		{#if visible}
			<div
				in:fly={{ y: 30, duration: 800, delay: 500 }}
				class="mb-20 grid grid-cols-2 gap-8 rounded-2xl border border-blue-100 bg-white p-8 shadow-lg md:grid-cols-4"
			>
				{#each stats as stat, i}
					<div class="text-center">
						<div class="mb-2 text-3xl font-bold text-blue-600 md:text-4xl">
							{Math.floor(counters[i].current)}{stat.suffix}
						</div>
						<div class="text-sm text-gray-500 md:text-base">{stat.label}</div>
					</div>
				{/each}
			</div>
		{/if}

		{#if visible}
			<div class="mb-20 grid grid-cols-1 gap-8 sm:grid-cols-2 lg:grid-cols-4">
				{#each benefits as benefit, i}
					<div
						in:fly={{ y: 30, duration: 800, delay: 600 + i * 100 }}
						class="group relative overflow-hidden rounded-xl border border-blue-100 bg-white p-6 transition-all duration-300 hover:border-blue-200 hover:shadow-xl"
					>
						<div
							class="absolute -top-10 -right-10 h-20 w-20 rounded-full bg-blue-50 transition-all duration-300 group-hover:bg-blue-100"
						></div>

						<div class="relative mb-4">
							<div
								class="flex h-14 w-14 items-center justify-center rounded-xl bg-blue-50 text-blue-600 transition-all duration-300 group-hover:bg-blue-100 group-hover:text-blue-700"
							>
								<svelte:component this={benefit.icon} size={24} />
							</div>
						</div>

						<h3 class="relative mb-3 text-xl font-bold text-gray-800">{benefit.title}</h3>
						<p class="relative text-gray-600">{benefit.description}</p>

						<div
							class="relative mt-4 flex items-center text-blue-600 opacity-0 transition-all duration-300 group-hover:opacity-100"
						>
							<span class="text-sm font-medium">Learn more</span>
							<ChevronRight size={16} class="ml-1" />
						</div>
					</div>
				{/each}
			</div>
		{/if}

		{#if visible}
			<div in:fade={{ duration: 800, delay: 1000 }} class="mb-20 py-10" id="school-partners">
				<div class="mb-12 text-center">
					<div
						in:scale={{ duration: 600, start: 0.8 }}
						class="mx-auto mb-4 inline-flex items-center rounded-full bg-blue-100 px-4 py-1.5 text-sm font-medium text-blue-700"
					>
						<span class="mr-2 flex h-2 w-2 rounded-full bg-blue-600"></span>
						Accredited Institutions
					</div>

					<h3
						in:fly={{ y: -20, duration: 800, delay: 200 }}
						class="mb-4 bg-blue-600 bg-clip-text text-3xl font-bold tracking-tight text-transparent md:text-4xl"
					>
						Our School Partners
					</h3>

					<p
						in:fly={{ y: 20, duration: 800, delay: 400 }}
						class="mx-auto max-w-2xl text-base text-gray-600"
					>
						We collaborate with these leading accredited institutions to provide you with the best
						educational opportunities.
					</p>
				</div>

				<div
					class="flex flex-wrap items-center justify-center gap-6 md:gap-8 lg:grid lg:grid-cols-4"
				>
					{#each partners as partner, i}
						<div
							in:scale={{ duration: 600, delay: 1000 + i * 100, start: 0.8 }}
							class="group flex flex-col items-center"
						>
							<div
								class="mb-3 flex h-20 w-40 items-center justify-center overflow-hidden rounded-lg border border-gray-100 bg-white p-4 shadow-sm transition-all duration-300 hover:border-blue-100 hover:shadow-md"
							>
								<img
									src={partner.logo}
									alt={partner.name}
									class="h-auto max-h-full scale-[1.1] object-contain transition-all duration-300"
								/>
							</div>
							<span class="w-[150px] text-center text-xs text-gray-500">{partner.name}</span>
						</div>
					{/each}
				</div>
			</div>
		{/if}

		{#if visible}
			<div
				in:fly={{ y: 30, duration: 800, delay: 1200 }}
				class="relative mx-auto max-w-4xl overflow-hidden rounded-2xl bg-gradient-to-r from-blue-600 to-indigo-600 p-1"
			>
				<div class="absolute -top-20 -left-20 h-40 w-40 rounded-full bg-blue-400/30 blur-3xl"></div>
				<div
					class="absolute -right-20 -bottom-20 h-40 w-40 rounded-full bg-indigo-400/30 blur-3xl"
				></div>

				<div class="relative rounded-xl bg-white p-8 sm:p-10">
					<div class="grid gap-8 md:grid-cols-5">
						<div class="md:col-span-3">
							<div class="mb-2 flex items-center">
								{#each Array(5) as _, i}
									<Star size={20} fill="#FFD700" color="#FFD700" class="mr-1" />
								{/each}
								<span class="ml-2 text-sm font-medium text-gray-600"
									>Rated 4.9/5 by our students</span
								>
							</div>

							<h3 class="mb-4 text-2xl font-bold text-gray-800 sm:text-3xl">
								Ready to Find Your Perfect School?
							</h3>
							<p class="mb-6 text-gray-600">
								Let us help you connect with top accredited institutions that match your educational
								goals and preferences. Our personalized matching system finds the best fit for your
								future.
							</p>

							<!-- Testimonial quote -->
							<div class="mb-6 rounded-lg border-l-4 border-blue-500 bg-blue-50 p-4">
								<p class="text-gray-700 italic">
									"School Match Pro helped me find the perfect university for my major. The process
									was simple and I'm now in my dream program!"
								</p>
								<p class="mt-2 text-sm font-medium text-gray-900">
									— Sarah Johnson, Psychology Major
								</p>
							</div>
						</div>

						<div class="md:col-span-2">
							<div class="rounded-xl bg-gray-50 p-6 shadow-inner">
								<h4 class="mb-4 text-center text-lg font-semibold text-gray-800">
									Join our newsletter
								</h4>

								<!-- Simple form -->
								<div class="space-y-4">
									<div>
										<label for="email" class="mb-1 block text-sm font-medium text-gray-700"
											>Email</label
										>
										<input
											type="email"
											id="email"
											placeholder="your@email.com"
											class="w-full rounded-lg border border-gray-300 p-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-200 focus:outline-none"
										/>
									</div>

									<div>
										<label
											for="education-level"
											class="mb-1 block text-sm font-medium text-gray-700">Education Level</label
										>
										<select
											id="education-level"
											class="w-full rounded-lg border border-gray-300 p-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-200 focus:outline-none"
										>
											<option value="">Select your education level</option>
											<option value="high-school">High School</option>
											<option value="associate">Associate Degree</option>
											<option value="bachelor">Bachelor's Degree</option>
											<option value="master">Master's Degree</option>
											<option value="doctorate">Doctorate</option>
										</select>
									</div>

									<button
										class="w-full cursor-pointer rounded-lg bg-gradient-to-r from-blue-600 to-indigo-600 py-3 font-medium text-white transition-all hover:shadow-lg"
									>
										Stay connected
									</button>

									<p class="text-center text-xs text-gray-500">
										By submitting, you agree to our Terms of Service and Privacy Policy
									</p>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		{/if}
	</div>
</section>

<style>
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

	@keyframes pulse {
		0% {
			box-shadow: 0 0 0 0 rgba(59, 130, 246, 0.4);
		}
		70% {
			box-shadow: 0 0 0 10px rgba(59, 130, 246, 0);
		}
		100% {
			box-shadow: 0 0 0 0 rgba(59, 130, 246, 0);
		}
	}

	:global(body) {
		margin: 0;
		font-family:
			system-ui,
			-apple-system,
			BlinkMacSystemFont,
			'Segoe UI',
			Roboto,
			Oxygen,
			Ubuntu,
			Cantarell,
			'Open Sans',
			'Helvetica Neue',
			sans-serif;
	}
</style>
