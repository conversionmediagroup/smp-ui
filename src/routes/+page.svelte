<script lang="ts">
	import { fade } from 'svelte/transition';

	import Header from './components/Header/Header.svelte';

	// Form state
	let formData = {
		firstName: '',
		lastName: '',
		email: '',
		phone: '',
		program: '',
		educationLevel: '',
		startDate: ''
	};

	let formSubmitted = false;
	let tcpaAgreed = false;

	// Handle form submission
	function handleSubmit() {
		if (!tcpaAgreed) {
			alert('Please agree to the terms and conditions');
			return;
		}

		console.log('Form submitted:', formData);
		formSubmitted = true;
		// In a real implementation, you would send this data to your backend/CRM
	}

	// Navigation items
	const navItems = [
		{ name: 'Home', href: '#' },
		{ name: 'Programs', href: '#' },
		{ name: 'About Us', href: '#' },
		{ name: 'Contact', href: '#' }
	];

	// Program options
	const programOptions = [
		'Business Administration',
		'Computer Science',
		'Education',
		'Healthcare',
		'Liberal Arts',
		'Engineering'
	];

	// Education level options
	const educationLevels = [
		'High School Diploma',
		'Some College',
		"Associate's Degree",
		"Bachelor's Degree",
		"Master's Degree",
		'Doctorate'
	];

	const startDates = ['As soon as possible', 'Within 3 months', 'Within 6 months', 'Next year'];

	let mobileMenuOpen = false;

	function toggleMobileMenu() {
		mobileMenuOpen = !mobileMenuOpen;
	}
</script>

<div class="flex min-h-screen flex-col bg-white">
	<section class="bg-white py-12 md:py-20">
		<div class="flex flex-col items-center gap-12 lg:flex-row">
			<div class="space-y-14 text-center lg:w-1/2 lg:text-left">
				<div class="space-y-8">
					<h1
						class="px-10 text-3xl leading-12 tracking-tighter text-black/80 md:text-4xl lg:text-5xl"
					>
						Find Your Perfect School with School Match Pro
					</h1>

					<p class="text-2xl tracking-tighter text-black/80 md:text-4xl lg:text-5xl">
						Your Gateway to a Brighter Future!
					</p>
				</div>

				<div class="space-y-8">
					<p class="mx-auto mt-6 w-[300px] text-center leading-5 tracking-tighter text-gray-600">
						Connect with top accredited colleges and universities that match your educational goals
						and preferences. To get started, we learn about you with a few quick questions!
					</p>

					<button
						class="loading w-[50vw] max-w-[200px] cursor-pointer self-center rounded-full py-1.5 text-white shadow-md transition-all duration-300 hover:scale-[1.01]"
					>
						<span class="relative z-10 text-base font-medium text-white"> Get Started </span>
					</button>
				</div>

				<!-- Trust Indicators -->
				<!-- <div class="mt-8 grid grid-cols-3 gap-4 text-center">
						<div>
							<div class="text-2xl font-bold text-blue-600">500+</div>
							<div class="text-sm text-gray-500">Partner Schools</div>
						</div>
						<div>
							<div class="text-2xl font-bold text-blue-600">98%</div>
							<div class="text-sm text-gray-500">Satisfaction Rate</div>
						</div>
						<div>
							<div class="text-2xl font-bold text-blue-600">50k+</div>
							<div class="text-sm text-gray-500">Students Matched</div>
						</div>
					</div> -->
			</div>
		</div>
	</section>

	<footer class="mt-auto bg-gray-800 pt-12 pb-8 text-white">
		<div class="container mx-auto px-4">
			<div class="grid grid-cols-1 gap-8 md:grid-cols-2 lg:grid-cols-4">
				<div>
					<h3 class="mb-4 text-lg font-bold">SchoolMatchPro</h3>
					<p class="mb-4 text-gray-400">
						Connecting prospective students with top accredited colleges and universities across the
						United States.
					</p>
					<div class="flex space-x-4">
						<a href="#" class="text-gray-400 hover:text-white">
							<span class="sr-only">Facebook</span>
							<svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
								<path
									fill-rule="evenodd"
									d="M22 12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33v6.988C18.343 21.128 22 16.991 22 12z"
									clip-rule="evenodd"
								/>
							</svg>
						</a>
						<a href="#" class="text-gray-400 hover:text-white">
							<span class="sr-only">Twitter</span>
							<svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
								<path
									d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84"
								/>
							</svg>
						</a>
						<a href="#" class="text-gray-400 hover:text-white">
							<span class="sr-only">LinkedIn</span>
							<svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
								<path
									fill-rule="evenodd"
									d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"
									clip-rule="evenodd"
								/>
							</svg>
						</a>
					</div>
				</div>

				<!-- Quick Links -->
				<div>
					<h3 class="mb-4 text-lg font-bold">Quick Links</h3>
					<ul class="space-y-2">
						<li><a href="#" class="text-gray-400 hover:text-white">Home</a></li>
						<li><a href="#" class="text-gray-400 hover:text-white">About Us</a></li>
						<li><a href="#" class="text-gray-400 hover:text-white">Programs</a></li>
						<li><a href="#" class="text-gray-400 hover:text-white">Partner Schools</a></li>
						<li><a href="#" class="text-gray-400 hover:text-white">Contact</a></li>
					</ul>
				</div>

				<!-- Resources -->
				<div>
					<h3 class="mb-4 text-lg font-bold">Resources</h3>
					<ul class="space-y-2">
						<li><a href="#" class="text-gray-400 hover:text-white">Student Resources</a></li>
						<li><a href="#" class="text-gray-400 hover:text-white">Financial Aid</a></li>
						<li><a href="#" class="text-gray-400 hover:text-white">Career Guidance</a></li>
						<li><a href="#" class="text-gray-400 hover:text-white">Blog</a></li>
						<li><a href="#" class="text-gray-400 hover:text-white">FAQ</a></li>
					</ul>
				</div>

				<!-- Contact -->
				<div>
					<h3 class="mb-4 text-lg font-bold">Contact Us</h3>
					<ul class="space-y-2 text-gray-400">
						<li>
							<a href="mailto:info@schoolmatchpro.com" class="hover:text-white"
								>info@schoolmatchpro.com</a
							>
						</li>
						<li>
							<a href="tel:+18005551234" class="hover:text-white">(800) 555-1234</a>
						</li>
						<li>
							123 Education Lane<br />
							Suite 100<br />
							San Francisco, CA 94107
						</li>
					</ul>
				</div>
			</div>

			<div class="mt-8 border-t border-gray-700 pt-8 text-center text-sm text-gray-400">
				<p>&copy; {new Date().getFullYear()} SchoolMatchPro. All rights reserved.</p>
				<div class="mt-2 space-x-4">
					<a href="#" class="hover:text-white">Privacy Policy</a>
					<a href="#" class="hover:text-white">Terms of Service</a>
					<a href="#" class="hover:text-white">TCPA Consent</a>
				</div>
			</div>
		</div>
	</footer>
</div>

<style>
	:global(body) {
		font-family: 'Inter', sans-serif;
		margin: 0;
		padding: 0;
	}
</style>
