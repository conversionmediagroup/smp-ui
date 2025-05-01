<script lang="ts">
	import { onMount, tick } from 'svelte';
	import { fade, fly, scale } from 'svelte/transition';
	import { quintOut, cubicInOut } from 'svelte/easing';
	import {
		ChevronRight,
		ChevronLeft,
		Check,
		ArrowRight,
		Shield,
		Sparkles,
		GraduationCap,
		BookOpen,
		Globe,
		Home,
		User,
		Flag,
		Wifi,
		Calendar,
		MapPin
	} from 'lucide-svelte';
	import confetti from 'canvas-confetti';
	import Header from '../components/Header/Header.svelte';
	import Footer from '../components/Footer/Footer.svelte';

	// Form state management
	let currentStep = 0;
	let formData = {
		// Program & Education
		programArea: '',
		educationLevel: '',
		degreeType: '',
		graduationYear: '',

		// Learning Preferences
		startTimeframe: '',
		learningEnvironment: '',
		timeToStart: '',
		internetAccess: '',

		// Personal Information
		ageRange: '',
		citizenship: '',
		militaryStatus: '',
		militaryBranch: '',
		zipCode: '',

		// Contact Information
		firstName: '',
		lastName: '',
		email: '',
		phone: '',
		address: '',
		city: '',
		state: ''
	};

	// Form validation
	let errors = {};
	let touched = {};

	// Steps configuration
	const steps = [
		{
			id: 'welcome',
			title: 'Welcome',
			description: 'Find your perfect educational match with our personalized school finder.',
			icon: GraduationCap,
			fields: []
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

	// Animation and interaction state
	let visible = false;
	let formSubmitted = false;
	let formSubmitting = false;
	let showWelcome = true;

	// Program areas
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

	// Education level options
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

	// Degree types
	const degreeTypes = [
		'Certificate',
		'Associate (AA, AS)',
		"Bachelor's (BA, BS)",
		"Master's (MA, MS, MBA)",
		'Doctoral (PhD, EdD)',
		'Professional (JD, MD)',
		'Not Sure Yet'
	];

	// Learning environment options
	const learningEnvironments = ['On-Campus', 'Online', 'Hybrid', 'No Preference'];

	// Start timeframe options
	const startTimeframes = [
		'Immediately',
		'Within 3 months',
		'Within 6 months',
		'Next year',
		'Just exploring options'
	];

	// Time to start options
	const timeToStartOptions = [
		'Full-time',
		'Part-time',
		'Evenings/Weekends',
		'Flexible',
		'Not Sure'
	];

	// Internet access options
	const internetAccessOptions = [
		'High-speed reliable internet',
		'Moderate internet access',
		'Limited internet access',
		'No reliable internet access'
	];

	// Age range options
	const ageRanges = ['Under 18', '18-24', '25-34', '35-44', '45-54', '55+'];

	// Citizenship options
	const citizenshipOptions = [
		'U.S. Citizen',
		'U.S. Permanent Resident',
		'International Student',
		'Other'
	];

	// Military status options
	const militaryStatusOptions = [
		'Not affiliated with military',
		'Active Duty',
		'Veteran',
		'Reserves/National Guard',
		'Military Spouse/Dependent'
	];

	// Military branch options
	const militaryBranchOptions = [
		'Army',
		'Navy',
		'Air Force',
		'Marines',
		'Coast Guard',
		'Space Force'
	];

	// States
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
	$: showMilitaryBranch =
		formData.militaryStatus && formData.militaryStatus !== 'Not affiliated with military';

	// Validate current step
	function validateCurrentStep() {
		// Skip validation for welcome step
		if (currentStep === 0) return true;

		const currentFields = steps[currentStep].fields;
		let isValid = true;
		let newErrors = {};

		currentFields.forEach((field) => {
			// Skip militaryBranch validation if not military
			if (field === 'militaryBranch' && !showMilitaryBranch) {
				return;
			}

			if (!formData[field] || formData[field].trim() === '') {
				newErrors[field] = 'This field is required';
				isValid = false;
			} else if (field === 'email' && !isValidEmail(formData.email)) {
				newErrors.email = 'Please enter a valid email address';
				isValid = false;
			} else if (field === 'phone' && !isValidPhone(formData.phone)) {
				newErrors.phone = 'Please enter a valid phone number';
				isValid = false;
			} else if (field === 'zipCode' && !isValidZipCode(formData.zipCode)) {
				newErrors.zipCode = 'Please enter a valid ZIP code';
				isValid = false;
			}
		});

		errors = newErrors;
		return isValid;
	}

	// Email validation
	function isValidEmail(email) {
		return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
	}

	// Phone validation
	function isValidPhone(phone) {
		return /^[\d\s\-()]{10,15}$/.test(phone);
	}

	// ZIP code validation
	function isValidZipCode(zipCode) {
		return /^\d{5}(-\d{4})?$/.test(zipCode);
	}

	// Mark field as touched
	function handleBlur(field) {
		touched[field] = true;
	}

	// Start the form (from welcome screen)
	function startForm() {
		showWelcome = false;
		nextStep();
	}

	// Go to next step
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
		}
	}

	function goToStep(index) {
		// Only allow going to completed steps or the next available step
		if (index <= currentStep || (validateCurrentStep() && index === currentStep + 1)) {
			currentStep = index;
		}
	}

	async function submitForm() {
		if (validateCurrentStep()) {
			formSubmitting = true;

			await new Promise((resolve) => setTimeout(resolve, 1500));

			formSubmitting = false;
			formSubmitted = true;

			setTimeout(() => {
				const confettiCanvas = document.getElementById('confetti-canvas');
				if (confettiCanvas) {
					const myConfetti = confetti.create(confettiCanvas, {
						resize: true,
						useWorker: true
					});

					myConfetti({
						particleCount: 100,
						spread: 70,
						origin: { y: 0.6 }
					});

					setTimeout(() => {
						myConfetti({
							particleCount: 50,
							angle: 60,
							spread: 55,
							origin: { x: 0 }
						});
					}, 250);

					setTimeout(() => {
						myConfetti({
							particleCount: 50,
							angle: 120,
							spread: 55,
							origin: { x: 1 }
						});
					}, 400);
				}
			}, 300);
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

	{#if visible && !formSubmitted}
		<div in:fade={{ duration: 800 }} class="mb-8 text-center">
			<div
				class="mx-auto mb-4 inline-flex items-center justify-center rounded-full bg-blue-100 px-4 py-2 text-blue-700"
			>
				<GraduationCap size={18} class="mr-2" />
				<span class="text-sm font-semibold">Find Your Perfect School Match</span>
			</div>
			<h2 class="mb-4 text-3xl font-bold tracking-tight text-gray-900 md:text-4xl">
				Your Educational Journey <span class="text-blue-600">Starts Here</span>
			</h2>
			<p class="mx-auto max-w-2xl text-lg text-gray-600">
				Complete this short form and we'll match you with schools that align with your goals,
				preferences, and academic background.
			</p>
		</div>
	{/if}

	{#if visible && !formSubmitted && !showWelcome && currentStep > 0}
		<div in:fly={{ y: 20, duration: 600 }} class="mx-auto mb-10 max-w-[1200px]">
			<div class="mb-2 flex items-center justify-between">
				<span class="text-sm font-medium text-gray-500"
					>Step {currentStep} of {steps.length - 1}</span
				>
				<span class="text-sm font-medium text-blue-600"
					>{Math.round(progressPercentage)}% Complete</span
				>
			</div>

			<div class="h-2 w-full overflow-hidden rounded-full bg-gray-200">
				<div
					class="h-full rounded-full bg-blue-600 transition-all duration-500 ease-out"
					style="width: {progressPercentage}%"
				></div>
			</div>

			<div class="mt-6 hidden justify-between md:flex">
				{#each steps.slice(1) as step, index}
					<button on:click={() => goToStep(index + 1)} class="group flex flex-col items-center">
						<div
							class="relative mb-2 flex h-10 w-10 items-center justify-center rounded-full border-2 transition-all duration-200 {index +
								1 <
							currentStep
								? 'border-blue-600 bg-blue-600 text-white'
								: index + 1 === currentStep
									? 'border-blue-600 bg-white text-blue-600'
									: 'border-gray-300 bg-white text-gray-400'}"
						>
							{#if index + 1 < currentStep}
								<Check size={18} />
							{:else}
								<span>{index + 1}</span>
							{/if}

							{#if index < steps.length - 2}
								<div
									class="absolute top-1/2 left-10 h-[2px] w-12 -translate-y-1/2 bg-gray-300 md:w-24 lg:w-32 {index +
										1 <
									currentStep
										? 'bg-blue-600'
										: 'bg-gray-300'}"
								></div>
							{/if}
						</div>
						<span
							class="text-center text-xs font-medium {index + 1 <= currentStep
								? 'text-blue-600'
								: 'text-gray-500'}">{step.title}</span
						>
					</button>
				{/each}
			</div>
		</div>
	{/if}

	{#if visible && !formSubmitted}
		<div
			class="mx-auto max-w-[1200px] rounded-2xl border border-gray-200 bg-white p-6 shadow-sm md:p-8"
		>
			{#if currentStep === 0 && showWelcome}
				<div in:fly={{ y: 20, duration: 300, delay: 200 }} class="w-full py-8 text-center">
					<div
						class="mx-auto mb-6 flex h-24 w-24 items-center justify-center rounded-full bg-blue-100"
					>
						<GraduationCap size={48} class="text-blue-600" />
					</div>
					<h3 class="mb-4 text-2xl font-bold text-gray-900">Welcome to School Match Pro</h3>
					<p class="mx-auto mb-8 max-w-lg text-gray-600">
						We're excited to help you find the perfect educational path. This quick questionnaire
						will help us understand your needs and match you with ideal programs.
					</p>
					<button
						on:click={startForm}
						class="inline-flex cursor-pointer items-center justify-center rounded-lg bg-blue-600 px-8 py-3 text-base font-medium text-white shadow-md transition-all hover:bg-blue-700 hover:shadow-lg focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:outline-none"
					>
						Get Started
						<ChevronRight size={18} class="ml-2" />
					</button>
				</div>
			{:else}
				{#each steps as step, index}
					{#if currentStep === index && (!showWelcome || index > 0)}
						<div
							in:fly={{ y: 20, duration: 300, delay: 200 }}
							out:fade={{ duration: 200 }}
							class="w-full"
						>
							<div class="mb-6 flex items-start md:items-center">
								<div class="mr-4 hidden rounded-full bg-blue-100 p-3 text-blue-600 md:block">
									<svelte:component this={step.icon} size={24} />
								</div>
								<div>
									<h3 class="text-xl font-bold text-gray-900">{step.title}</h3>
									<p class="text-gray-600">{step.description}</p>
								</div>
							</div>

							<div class="grid gap-6 md:grid-cols-2">
								{#if step.id === 'program'}
									<!-- Program & Education Fields -->
									<div class="form-group md:col-span-2">
										<label for="programArea" class="mb-2 block text-sm font-medium text-gray-700"
											>What do you want to study?</label
										>
										<select
											id="programArea"
											bind:value={formData.programArea}
											on:blur={() => handleBlur('programArea')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.programArea &&
											touched.programArea
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select program area</option>
											{#each programAreas as area}
												<option value={area}>{area}</option>
											{/each}
										</select>
										{#if errors.programArea && touched.programArea}
											<p class="mt-1 text-sm text-red-500">{errors.programArea}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="educationLevel" class="mb-2 block text-sm font-medium text-gray-700"
											>Highest education level achieved</label
										>
										<select
											id="educationLevel"
											bind:value={formData.educationLevel}
											on:blur={() => handleBlur('educationLevel')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.educationLevel &&
											touched.educationLevel
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select your education level</option>
											{#each educationLevels as level}
												<option value={level}>{level}</option>
											{/each}
										</select>
										{#if errors.educationLevel && touched.educationLevel}
											<p class="mt-1 text-sm text-red-500">{errors.educationLevel}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="degreeType" class="mb-2 block text-sm font-medium text-gray-700"
											>Type of Degree</label
										>
										<select
											id="degreeType"
											bind:value={formData.degreeType}
											on:blur={() => handleBlur('degreeType')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.degreeType &&
											touched.degreeType
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select degree type</option>
											{#each degreeTypes as type}
												<option value={type}>{type}</option>
											{/each}
										</select>
										{#if errors.degreeType && touched.degreeType}
											<p class="mt-1 text-sm text-red-500">{errors.degreeType}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="graduationYear" class="mb-2 block text-sm font-medium text-gray-700"
											>Graduation Year</label
										>
										<input
											type="text"
											id="graduationYear"
											bind:value={formData.graduationYear}
											on:blur={() => handleBlur('graduationYear')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.graduationYear &&
											touched.graduationYear
												? 'border-red-500'
												: ''}"
											placeholder="e.g., 2022"
										/>
										{#if errors.graduationYear && touched.graduationYear}
											<p class="mt-1 text-sm text-red-500">{errors.graduationYear}</p>
										{/if}
									</div>
								{:else if step.id === 'learning'}
									<!-- Learning Preferences Fields -->
									<div class="form-group">
										<label for="startTimeframe" class="mb-2 block text-sm font-medium text-gray-700"
											>Desired start timeframe</label
										>
										<select
											id="startTimeframe"
											bind:value={formData.startTimeframe}
											on:blur={() => handleBlur('startTimeframe')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.startTimeframe &&
											touched.startTimeframe
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select start timeframe</option>
											{#each startTimeframes as timeframe}
												<option value={timeframe}>{timeframe}</option>
											{/each}
										</select>
										{#if errors.startTimeframe && touched.startTimeframe}
											<p class="mt-1 text-sm text-red-500">{errors.startTimeframe}</p>
										{/if}
									</div>

									<div class="form-group">
										<label
											for="learningEnvironment"
											class="mb-2 block text-sm font-medium text-gray-700"
											>Preferred learning environment</label
										>
										<select
											id="learningEnvironment"
											bind:value={formData.learningEnvironment}
											on:blur={() => handleBlur('learningEnvironment')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.learningEnvironment &&
											touched.learningEnvironment
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select learning environment</option>
											{#each learningEnvironments as environment}
												<option value={environment}>{environment}</option>
											{/each}
										</select>
										{#if errors.learningEnvironment && touched.learningEnvironment}
											<p class="mt-1 text-sm text-red-500">{errors.learningEnvironment}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="timeToStart" class="mb-2 block text-sm font-medium text-gray-700"
											>Time to Start</label
										>
										<select
											id="timeToStart"
											bind:value={formData.timeToStart}
											on:blur={() => handleBlur('timeToStart')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.timeToStart &&
											touched.timeToStart
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select time commitment</option>
											{#each timeToStartOptions as option}
												<option value={option}>{option}</option>
											{/each}
										</select>
										{#if errors.timeToStart && touched.timeToStart}
											<p class="mt-1 text-sm text-red-500">{errors.timeToStart}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="internetAccess" class="mb-2 block text-sm font-medium text-gray-700"
											>Internet Access</label
										>
										<select
											id="internetAccess"
											bind:value={formData.internetAccess}
											on:blur={() => handleBlur('internetAccess')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.internetAccess &&
											touched.internetAccess
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select internet access level</option>
											{#each internetAccessOptions as option}
												<option value={option}>{option}</option>
											{/each}
										</select>
										{#if errors.internetAccess && touched.internetAccess}
											<p class="mt-1 text-sm text-red-500">{errors.internetAccess}</p>
										{/if}
									</div>
								{:else if step.id === 'personal'}
									<!-- Personal Information Fields -->
									<div class="form-group">
										<label for="ageRange" class="mb-2 block text-sm font-medium text-gray-700"
											>Age Range</label
										>
										<select
											id="ageRange"
											bind:value={formData.ageRange}
											on:blur={() => handleBlur('ageRange')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.ageRange &&
											touched.ageRange
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select age range</option>
											{#each ageRanges as range}
												<option value={range}>{range}</option>
											{/each}
										</select>
										{#if errors.ageRange && touched.ageRange}
											<p class="mt-1 text-sm text-red-500">{errors.ageRange}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="citizenship" class="mb-2 block text-sm font-medium text-gray-700"
											>Citizenship</label
										>
										<select
											id="citizenship"
											bind:value={formData.citizenship}
											on:blur={() => handleBlur('citizenship')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.citizenship &&
											touched.citizenship
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select citizenship status</option>
											{#each citizenshipOptions as option}
												<option value={option}>{option}</option>
											{/each}
										</select>
										{#if errors.citizenship && touched.citizenship}
											<p class="mt-1 text-sm text-red-500">{errors.citizenship}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="militaryStatus" class="mb-2 block text-sm font-medium text-gray-700"
											>Military Status</label
										>
										<select
											id="militaryStatus"
											bind:value={formData.militaryStatus}
											on:blur={() => handleBlur('militaryStatus')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.militaryStatus &&
											touched.militaryStatus
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select military status</option>
											{#each militaryStatusOptions as option}
												<option value={option}>{option}</option>
											{/each}
										</select>
										{#if errors.militaryStatus && touched.militaryStatus}
											<p class="mt-1 text-sm text-red-500">{errors.militaryStatus}</p>
										{/if}
									</div>

									{#if showMilitaryBranch}
										<div class="form-group">
											<label
												for="militaryBranch"
												class="mb-2 block text-sm font-medium text-gray-700">Military Branch</label
											>
											<select
												id="militaryBranch"
												bind:value={formData.militaryBranch}
												on:blur={() => handleBlur('militaryBranch')}
												class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.militaryBranch &&
												touched.militaryBranch
													? 'border-red-500'
													: ''}"
											>
												<option value="">Select military branch</option>
												{#each militaryBranchOptions as option}
													<option value={option}>{option}</option>
												{/each}
											</select>
											{#if errors.militaryBranch && touched.militaryBranch}
												<p class="mt-1 text-sm text-red-500">{errors.militaryBranch}</p>
											{/if}
										</div>
									{/if}

									<div class="form-group">
										<label for="zipCode" class="mb-2 block text-sm font-medium text-gray-700"
											>ZIP Code</label
										>
										<input
											type="text"
											id="zipCode"
											bind:value={formData.zipCode}
											on:blur={() => handleBlur('zipCode')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.zipCode &&
											touched.zipCode
												? 'border-red-500'
												: ''}"
											placeholder="e.g., 12345"
										/>
										{#if errors.zipCode && touched.zipCode}
											<p class="mt-1 text-sm text-red-500">{errors.zipCode}</p>
										{/if}
									</div>
								{:else if step.id === 'contact'}
									<!-- Contact Information Fields -->
									<div class="form-group">
										<label for="firstName" class="mb-2 block text-sm font-medium text-gray-700"
											>First Name</label
										>
										<input
											type="text"
											id="firstName"
											bind:value={formData.firstName}
											on:blur={() => handleBlur('firstName')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.firstName &&
											touched.firstName
												? 'border-red-500'
												: ''}"
											placeholder="Enter your first name"
										/>
										{#if errors.firstName && touched.firstName}
											<p class="mt-1 text-sm text-red-500">{errors.firstName}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="lastName" class="mb-2 block text-sm font-medium text-gray-700"
											>Last Name</label
										>
										<input
											type="text"
											id="lastName"
											bind:value={formData.lastName}
											on:blur={() => handleBlur('lastName')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.lastName &&
											touched.lastName
												? 'border-red-500'
												: ''}"
											placeholder="Enter your last name"
										/>
										{#if errors.lastName && touched.lastName}
											<p class="mt-1 text-sm text-red-500">{errors.lastName}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="email" class="mb-2 block text-sm font-medium text-gray-700"
											>Email Address</label
										>
										<input
											type="email"
											id="email"
											bind:value={formData.email}
											on:blur={() => handleBlur('email')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.email &&
											touched.email
												? 'border-red-500'
												: ''}"
											placeholder="you@example.com"
										/>
										{#if errors.email && touched.email}
											<p class="mt-1 text-sm text-red-500">{errors.email}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="phone" class="mb-2 block text-sm font-medium text-gray-700"
											>Phone Number</label
										>
										<input
											type="tel"
											id="phone"
											bind:value={formData.phone}
											on:blur={() => handleBlur('phone')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.phone &&
											touched.phone
												? 'border-red-500'
												: ''}"
											placeholder="(123) 456-7890"
										/>
										{#if errors.phone && touched.phone}
											<p class="mt-1 text-sm text-red-500">{errors.phone}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="address" class="mb-2 block text-sm font-medium text-gray-700"
											>Street Address</label
										>
										<input
											type="text"
											id="address"
											bind:value={formData.address}
											on:blur={() => handleBlur('address')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.address &&
											touched.address
												? 'border-red-500'
												: ''}"
											placeholder="123 Main St"
										/>
										{#if errors.address && touched.address}
											<p class="mt-1 text-sm text-red-500">{errors.address}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="city" class="mb-2 block text-sm font-medium text-gray-700"
											>City</label
										>
										<input
											type="text"
											id="city"
											bind:value={formData.city}
											on:blur={() => handleBlur('city')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.city &&
											touched.city
												? 'border-red-500'
												: ''}"
											placeholder="Anytown"
										/>
										{#if errors.city && touched.city}
											<p class="mt-1 text-sm text-red-500">{errors.city}</p>
										{/if}
									</div>

									<div class="form-group">
										<label for="state" class="mb-2 block text-sm font-medium text-gray-700"
											>State</label
										>
										<select
											id="state"
											bind:value={formData.state}
											on:blur={() => handleBlur('state')}
											class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.state &&
											touched.state
												? 'border-red-500'
												: ''}"
										>
											<option value="">Select state</option>
											{#each states as state}
												<option value={state}>{state}</option>
											{/each}
										</select>
										{#if errors.state && touched.state}
											<p class="mt-1 text-sm text-red-500">{errors.state}</p>
										{/if}
									</div>
								{/if}
							</div>
						</div>
					{/if}
				{/each}
			{/if}

			<!-- Navigation buttons -->
			{#if !showWelcome}
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

	<!-- Success message after form submission with confetti -->
	{#if formSubmitted}
		<div
			in:scale={{ duration: 400, delay: 200, easing: quintOut }}
			class="relative rounded-2xl border border-green-100 bg-white p-8 text-center shadow-sm"
		>
			<!-- Canvas for confetti -->
			<canvas id="confetti-canvas" class="pointer-events-none absolute inset-0 h-full w-full"
			></canvas>

			<div
				class="relative z-10 mx-auto mb-4 flex h-16 w-16 items-center justify-center rounded-full bg-green-100"
			>
				<Check size={32} class="text-green-600" />
			</div>
			<h3 class="relative z-10 mb-2 text-2xl font-bold text-gray-900">Congratulations!</h3>
			<p class="relative z-10 mb-6 text-gray-600">
				Thank you for completing your profile. We've found <span class="font-semibold text-blue-600"
					>3 schools</span
				> that match your preferences! You'll receive an email with your personalized recommendations
				within 24 hours.
			</p>

			<!-- Results preview -->
			<div class="relative z-10 mb-8 rounded-xl bg-blue-50 p-6">
				<h4 class="mb-4 text-lg font-semibold text-blue-800">Your Matches Preview</h4>
				<div class="grid gap-4 md:grid-cols-3">
					<div class="rounded-lg bg-white p-4 shadow-sm">
						<div
							class="mx-auto mb-2 flex h-12 w-12 items-center justify-center rounded-full bg-blue-100"
						>
							<GraduationCap size={24} class="text-blue-600" />
						</div>
						<h5 class="font-medium text-gray-900">University of Excellence</h5>
						<p class="text-sm text-gray-500">98% Match</p>
					</div>
					<div class="rounded-lg bg-white p-4 shadow-sm">
						<div
							class="mx-auto mb-2 flex h-12 w-12 items-center justify-center rounded-full bg-blue-100"
						>
							<BookOpen size={24} class="text-blue-600" />
						</div>
						<h5 class="font-medium text-gray-900">Tech Institute</h5>
						<p class="text-sm text-gray-500">95% Match</p>
					</div>
					<div class="rounded-lg bg-white p-4 shadow-sm">
						<div
							class="mx-auto mb-2 flex h-12 w-12 items-center justify-center rounded-full bg-blue-100"
						>
							<Globe size={24} class="text-blue-600" />
						</div>
						<h5 class="font-medium text-gray-900">Global College</h5>
						<p class="text-sm text-gray-500">92% Match</p>
					</div>
				</div>
			</div>

			<div
				class="relative z-10 flex flex-col justify-center space-y-3 sm:flex-row sm:space-y-0 sm:space-x-3"
			>
				<a
					href="/"
					class="inline-flex items-center justify-center rounded-lg border border-gray-300 bg-white px-5 py-3 text-sm font-medium text-gray-700 shadow-sm transition-colors hover:bg-gray-50"
				>
					Return Home
				</a>
			</div>
		</div>
	{/if}

	<!-- Trust indicators -->
	{#if visible && !formSubmitted}
		<div
			in:fade={{ duration: 800, delay: 400 }}
			class="mx-auto mt-8 max-w-[1200px] rounded-xl border border-gray-200 bg-gray-50 p-6"
		>
			<div
				class="flex flex-col items-center justify-between gap-4 text-center md:flex-row md:text-left"
			>
				<div class="flex items-center">
					<Shield size={20} class="mr-2 text-blue-600" />
					<span class="text-sm font-medium text-gray-700"
						>Your information is secure and encrypted</span
					>
				</div>
				<div class="flex items-center">
					<Check size={20} class="mr-2 text-blue-600" />
					<span class="text-sm font-medium text-gray-700">100% satisfaction guarantee</span>
				</div>
				<div class="flex items-center">
					<Sparkles size={20} class="mr-2 text-blue-600" />
					<span class="text-sm font-medium text-gray-700"
						>Matched with accredited institutions only</span
					>
				</div>
			</div>
		</div>
	{/if}
</section>

<Footer />

<style>
	/* Optional: Add these animations if not already in your global styles */
	@keyframes pulse {
		0%,
		100% {
			opacity: 1;
		}
		50% {
			opacity: 0.5;
		}
	}

	.form-group {
		margin-bottom: 1.5rem;
	}

	/* Smooth transition for progress bar */
	.progress-bar-transition {
		transition: width 0.5s ease-out;
	}

	/* Focus styles for form elements */
	input:focus,
	select:focus,
	textarea:focus {
		outline: none;
		border-color: #2563eb;
		box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.1);
	}

	/* Hover effect for buttons */
	button:hover:not(:disabled) {
		transform: translateY(-1px);
	}

	button:active:not(:disabled) {
		transform: translateY(0);
	}
</style>
