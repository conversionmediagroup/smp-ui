<script lang="ts">
	import { onMount, tick } from 'svelte';
	import {
		ChevronRight,
		ChevronLeft,
		GraduationCap,
		BookOpen,
		Globe,
		Home,
		User
	} from 'lucide-svelte';

	import Header from '../components/Header/Header.svelte';
	import Footer from '../components/Footer/Footer.svelte';
	import FormHeader from './components/FormHeader.svelte';
	import ProgressBar from './components/ProgressBar.svelte';
	import WelcomeScreen from './components/WelcomeScreen.svelte';
	import FormStep from './components/FormStep.svelte';
	import SuccessMessage from './components/SuccessMessage.svelte';
	import TrustIndicators from './components/TrustIndicators.svelte';

	let currentStep = 0;
	let formData = {
		programArea: '',
		educationLevel: '',
		degreeType: '',
		graduationYear: '',
		startTimeframe: '',
		learningEnvironment: '',
		timeToStart: '',
		internetAccess: '',
		ageRange: '',
		citizenship: '',
		militaryStatus: '',
		militaryBranch: '',
		zipCode: '',
		firstName: '',
		lastName: '',
		email: '',
		phone: '',
		address: '',
		city: '',
		state: ''
	};

	let errors: { [key: string]: string } = {};
	let touched: { [key: string]: boolean } = {};

	const steps = [
		{
			id: 'welcome',
			title: 'Welcome',
			description: 'Find your perfect educational match with our personalized school finder.',
			icon: GraduationCap,
			fields: [] as string[]
		},
		{
			id: 'program',
			title: 'Program & Education',
			description: 'Tell us about your educational background and goals.',
			icon: BookOpen,
			fields: ['programArea', 'educationLevel', 'degreeType', 'graduationYear']
		},
		{
			id: 'learning',
			title: 'Learning Preferences',
			description: 'Help us understand how you prefer to learn.',
			icon: Globe,
			fields: ['startTimeframe', 'learningEnvironment', 'timeToStart', 'internetAccess']
		},
		{
			id: 'personal',
			title: 'Personal Information',
			description: 'Tell us a bit about yourself so we can personalize your experience.',
			icon: User,
			fields: ['ageRange', 'citizenship', 'militaryStatus', 'militaryBranch', 'zipCode']
		},
		{
			id: 'contact',
			title: 'Contact Information',
			description: 'How can schools reach you with information about their programs?',
			icon: Home,
			fields: ['firstName', 'lastName', 'email', 'phone', 'address', 'city', 'state']
		}
	];

	let visible = false;
	let formSubmitted = false;
	let formSubmitting = false;
	let showWelcome = true;

	const programAreas = [
		'Business & Management',
		'Computer Science & IT',
		'Education & Teaching',
		'Engineering',
		'Healthcare & Medicine',
		'Liberal Arts & Humanities',
		'Science & Mathematics',
		'Social Sciences',
		'Visual & Performing Arts',
		'Other'
	];

	const educationLevels = [
		'Some High School',
		'High School Diploma/GED',
		'Some College',
		'Associate Degree',
		"Bachelor's Degree",
		"Master's Degree",
		'Doctoral Degree',
		'Professional Degree',
		'Trade/Technical Training'
	];

	const degreeTypes = [
		'Certificate',
		'Associate (AA, AS)',
		"Bachelor's (BA, BS)",
		"Master's (MA, MS, MBA)",
		'Doctoral (PhD, EdD)',
		'Professional (JD, MD)',
		'Not Sure Yet'
	];

	const learningEnvironments = ['On-Campus', 'Online', 'Hybrid', 'No Preference'];

	const startTimeframes = [
		'Immediately',
		'Within 3 months',
		'Within 6 months',
		'Next year',
		'Just exploring options'
	];

	const timeToStartOptions = [
		'Full-time',
		'Part-time',
		'Evenings/Weekends',
		'Flexible',
		'Not Sure'
	];

	const internetAccessOptions = [
		'High-speed reliable internet',
		'Moderate internet access',
		'Limited internet access',
		'No reliable internet access'
	];

	const ageRanges = ['Under 18', '18-24', '25-34', '35-44', '45-54', '55+'];

	const citizenshipOptions = [
		'U.S. Citizen',
		'U.S. Permanent Resident',
		'International Student',
		'Other'
	];

	const militaryStatusOptions = [
		'Not affiliated with military',
		'Active Duty',
		'Veteran',
		'Reserves/National Guard',
		'Military Spouse/Dependent'
	];

	const militaryBranchOptions = [
		'Army',
		'Navy',
		'Air Force',
		'Marines',
		'Coast Guard',
		'Space Force'
	];

	const states = [
		'Alabama',
		'Alaska',
		'Arizona',
		'Arkansas',
		'California',
		'Colorado',
		'Connecticut',
		'Delaware',
		'Florida',
		'Georgia',
		'Hawaii',
		'Idaho',
		'Illinois',
		'Indiana',
		'Iowa',
		'Kansas',
		'Kentucky',
		'Louisiana',
		'Maine',
		'Maryland',
		'Massachusetts',
		'Michigan',
		'Minnesota',
		'Mississippi',
		'Missouri',
		'Montana',
		'Nebraska',
		'Nevada',
		'New Hampshire',
		'New Jersey',
		'New Mexico',
		'New York',
		'North Carolina',
		'North Dakota',
		'Ohio',
		'Oklahoma',
		'Oregon',
		'Pennsylvania',
		'Rhode Island',
		'South Carolina',
		'South Dakota',
		'Tennessee',
		'Texas',
		'Utah',
		'Vermont',
		'Virginia',
		'Washington',
		'West Virginia',
		'Wisconsin',
		'Wyoming'
	];

	// Calculate progress percentage
	$: progressPercentage = (currentStep / (steps.length - 1)) * 100;
	// Explicitly type showMilitaryBranch as boolean
	let showMilitaryBranch: boolean = false;
	$: showMilitaryBranch =
		!!formData.militaryStatus && formData.militaryStatus !== 'Not affiliated with military';

	// Validate current step
	function validateCurrentStep() {
		if (currentStep === 0) return true;

		const currentFields = steps[currentStep].fields;
		const newErrors: { [key: string]: string } = {};
		const newTouched: { [key: string]: boolean } = {};
		let isValid = true;

		currentFields.forEach((field) => {
			if (field === 'militaryBranch' && !showMilitaryBranch) {
				return;
			}

			// Check required
			if (!formData[field] || formData[field].trim() === '') {
				newErrors[field] = 'This field is required';
				newTouched[field] = true;
				isValid = false;
			} else {
				// Additional format validation
				if (field === 'graduationYear') {
					if (formData.graduationYear.length !== 4) {
						newErrors.graduationYear = 'Please enter a 4-digit year';
						newTouched.graduationYear = true;
						isValid = false;
					} else {
						const year = parseInt(formData.graduationYear, 10);
						if (year > 2025) {
							newErrors.graduationYear = 'Graduation year cannot be in the future';
							newTouched.graduationYear = true;
							isValid = false;
						} else if (year < 1900) {
							newErrors.graduationYear = 'Please enter a valid year';
							newTouched.graduationYear = true;
							isValid = false;
						}
					}
				} else if (field === 'email' && !isValidEmail(formData.email)) {
					newErrors.email = 'Please enter a valid email address';
					newTouched.email = true;
					isValid = false;
				} else if (field === 'phone' && !isValidPhone(formData.phone)) {
					newErrors.phone = 'Please enter a 10-digit phone number';
					newTouched.phone = true;
					isValid = false;
				} else if (field === 'zipCode' && !isValidZipCode(formData.zipCode)) {
					newErrors.zipCode = 'Please enter a 5-digit ZIP code';
					newTouched.zipCode = true;
					isValid = false;
				}
			}
		});

		errors = newErrors;
		touched = newTouched;
		return isValid;
	}

	function isValidEmail(email: string) {
		return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
	}

	function isValidPhone(phone: string) {
		return /^\d{10}$/.test(phone);
	}

	function isValidZipCode(zipCode: string) {
		return /^\d{5}$/.test(zipCode);
	}

	function handleBlur(_field: string) {
		// No-op: Remove blur validation
	}

	function startForm() {
		showWelcome = false;
		currentStep = 1;
	}

	async function nextStep() {
		if (validateCurrentStep()) {
			if (currentStep < steps.length - 1) {
				currentStep++;
				await tick();
				window.scrollTo({ top: 0, behavior: 'smooth' });
			} else {
				submitForm();
			}
		}
	}

	function prevStep() {
		if (currentStep > 0) {
			currentStep--;
			// Clear errors and touched when going back
			errors = {};
			touched = {};
		}
	}

	function goToStep(index: number) {
		if (index <= currentStep || (validateCurrentStep() && index === currentStep + 1)) {
			currentStep = index;
			// Clear errors and touched when navigating
			errors = {};
			touched = {};
		}
	}

	async function submitForm() {
		if (validateCurrentStep()) {
			formSubmitting = true;
			await new Promise((resolve) => setTimeout(resolve, 1500));
			formSubmitting = false;
			formSubmitted = true;
		}
	}

	onMount(() => {
		visible = true;
	});
</script>

<Header />

<section class="relative mx-auto overflow-hidden px-6 py-12 md:py-16" id="application-form">
	<div
		class="absolute top-0 right-0 -z-10 h-96 w-96 rounded-full bg-blue-50 opacity-70 blur-[100px]"
	></div>
	<div
		class="absolute bottom-0 left-0 -z-10 h-96 w-96 rounded-full bg-indigo-50 opacity-70 blur-[100px]"
	></div>

	<div
		class="absolute inset-0 -z-10 bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAiIGhlaWdodD0iNjAiIHZpZXdCb3g9IjAgMCA2MCA2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZyBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxnIGZpbGw9IiMwMDAiIGZpbGwtb3BhY2l0eT0iMSI+PHBhdGggZD0iTTM2IDM0YzAtMi4yMS0xLjc5LTQtNC00cy00IDEuNzktNCA0IDEuNzkgNCA0IDQgNC0xLjc5IDQtNHpNMjQgMzBjMC0yLjIxLTEuNzktNC00LTRzLTQgMS43OS00IDQgMi43OSA0IDQgNCA0LTEuNzkgNC00em0yNCAwYzAtMi4yMS0xLjc5LTQtNC00cy00IDEuNzktNCA0IDEuNzkgNCA0IDQgNC0xLjc5IDQtNHoiLz48L2c+PC9nPjwvc3ZnPg==')] opacity-[0.03]"
	></div>
	<FormHeader {visible} {formSubmitted} />

	<ProgressBar
		{currentStep}
		{steps}
		{progressPercentage}
		{goToStep}
		{visible}
		{showWelcome}
		{formSubmitted}
	/>

	{#if visible && !formSubmitted}
		<div
			class="mx-auto max-w-[1200px] rounded-2xl border border-gray-200 bg-white p-6 shadow-sm md:p-8"
		>
			{#if currentStep === 0 && showWelcome}
				<WelcomeScreen {startForm} />
			{:else}
				{#each steps as step, index}
					{#if currentStep === index && (!showWelcome || index > 0)}
						<FormStep
							{step}
							{formData}
							{errors}
							{touched}
							{handleBlur}
							{showMilitaryBranch}
							{programAreas}
							{educationLevels}
							{degreeTypes}
							{startTimeframes}
							{learningEnvironments}
							{timeToStartOptions}
							{internetAccessOptions}
							{ageRanges}
							{citizenshipOptions}
							{militaryStatusOptions}
							{militaryBranchOptions}
							{states}
						/>
					{/if}
				{/each}

				<div class="mt-8 flex justify-between">
					<button
						on:click={prevStep}
						class="inline-flex cursor-pointer items-center rounded-lg border border-gray-300 bg-white px-5 py-2.5 text-sm font-medium text-gray-700 shadow-sm transition-colors hover:bg-gray-50 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:outline-none disabled:opacity-50"
						disabled={currentStep === 0}
					>
						<ChevronLeft size={16} class="mr-1" />
						Previous
					</button>

					<button
						on:click={nextStep}
						class="inline-flex cursor-pointer items-center rounded-lg bg-blue-600 px-5 py-2.5 text-sm font-medium text-white shadow-sm transition-colors hover:bg-blue-700 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:outline-none disabled:opacity-50"
						disabled={formSubmitting}
					>
						{#if currentStep === steps.length - 1}
							{#if formSubmitting}
								<div
									class="mr-2 h-4 w-4 animate-spin rounded-full border-2 border-white border-t-transparent"
								></div>
								Submitting...
							{:else}
								Submit Application
							{/if}
						{:else}
							Next
							<ChevronRight size={16} class="ml-1" />
						{/if}
					</button>
				</div>
			{/if}
		</div>
	{/if}

	{#if formSubmitted}
		<div class="mx-auto max-w-[1200px]">
			<SuccessMessage />
		</div>
	{/if}

	<TrustIndicators {visible} {formSubmitted} />
</section>

<Footer />

<style>
	@keyframes pulse {
		0%,
		100% {
			opacity: 1;
		}
		50% {
			opacity: 0.5;
		}
	}

	.progress-bar-transition {
		transition: width 0.5s ease-out;
	}

	input:focus,
	select:focus,
	textarea:focus {
		outline: none;
		border-color: #2563eb;
		box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.1);
	}

	button:hover:not(:disabled) {
		transform: translateY(-1px);
	}

	button:active:not(:disabled) {
		transform: translateY(0);
	}
</style>
