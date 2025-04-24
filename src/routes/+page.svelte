<script lang="ts">
	import Header from './components/Header/Header.svelte';
	import PartnerScrollers from './components/Partners/PartnerScrollers.svelte';
	import Hero from './components/Hero/Hero.svelte';
	import Footer from './components/Footer/Footer.svelte';
	import Credits from './components/Partners/Credits.svelte';
	import FAQs from './components/FAQ/FAQs.svelte';
	import Test from './components/FAQ/Test.svelte';

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

	// const programOptions = [
	// 	'Business Administration',
	// 	'Computer Science',
	// 	'Education',
	// 	'Healthcare',
	// 	'Liberal Arts',
	// 	'Engineering'
	// ];

	// const educationLevels = [
	// 	'High School Diploma',
	// 	'Some College',
	// 	"Associate's Degree",
	// 	"Bachelor's Degree",
	// 	"Master's Degree",
	// 	'Doctorate'
	// ];

	// const startDates = ['As soon as possible', 'Within 3 months', 'Within 6 months', 'Next year'];
</script>

<div class="flex min-h-screen flex-col bg-slate-100">
	<Header />

	<Hero />

	<!-- <PartnerScrollers /> -->

	<Credits />

	<FAQs />

	<Footer />
</div>

<style>
	:global(body) {
		font-family: 'Inter', sans-serif;
		margin: 0;
		padding: 0;
	}
</style>
