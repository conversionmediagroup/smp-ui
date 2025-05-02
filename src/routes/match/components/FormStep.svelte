<script lang="ts">
	import { fly, fade } from 'svelte/transition';
	import type { ComponentType } from 'svelte';

	export let step: {
		id: string;
		title: string;
		description: string;
		icon: ComponentType;
		fields: string[];
	};
	export let formData: any;
	export let errors: any;
	export let touched: any;
	export let handleBlur: (field: string) => void;
	export let showMilitaryBranch: boolean;
	export let programAreas: string[];
	export let educationLevels: string[];
	export let degreeTypes: string[];
	export let startTimeframes: string[];
	export let learningEnvironments: string[];
	export let timeToStartOptions: string[];
	export let internetAccessOptions: string[];
	export let ageRanges: string[];
	export let citizenshipOptions: string[];
	export let militaryStatusOptions: string[];
	export let militaryBranchOptions: string[];
	export let states: string[];

	// Get the current year (as of May 01, 2025)
	const currentYear = 2025;

	// Function to restrict input to numbers only and enforce 4-digit limit for graduationYear
	function restrictToNumbers(event: Event) {
		const input = event.target as HTMLInputElement;
		const value = input.value;
		const numericValue = value.replace(/[^0-9]/g, '').slice(0, 4);
		if (formData.graduationYear !== numericValue) {
			formData.graduationYear = numericValue;
			// Clear error if value is now valid
			if (numericValue) {
				delete errors.graduationYear;
			}
		}
	}

	// Function to restrict input to numbers only and enforce 5-digit limit for zipCode
	function restrictToZipCodeNumbers(event: Event) {
		const input = event.target as HTMLInputElement;
		const value = input.value;
		const numericValue = value.replace(/[^0-9]/g, '').slice(0, 5);
		if (formData.zipCode !== numericValue) {
			formData.zipCode = numericValue;
			// Clear error if value is now valid
			if (numericValue) {
				delete errors.zipCode;
			}
		}
	}

	// Function to restrict input to numbers only and enforce 10-digit limit for phone
	function restrictToPhoneNumbers(event: Event) {
		const input = event.target as HTMLInputElement;
		const value = input.value;
		const numericValue = value.replace(/[^0-9]/g, '').slice(0, 10);
		if (formData.phone !== numericValue) {
			formData.phone = numericValue;
			// Clear error if value is now valid
			if (numericValue) {
				delete errors.phone;
			}
		}
	}

	// Improved function to handle input changes and clear errors
	function handleInputChange(field: string, value: any) {
		// Update the formData
		formData[field] = value;

		// Clear error if the field has a value
		if (value && (typeof value !== 'string' || value.trim() !== '')) {
			delete errors[field];
		}
	}
</script>

<div in:fly={{ y: 20, duration: 300, delay: 200 }} out:fade={{ duration: 200 }} class="w-full">
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
			<div class="form-group md:col-span-2">
				<label for="programArea" class="mb-2 block text-sm font-medium text-gray-700"
					>What do you want to study? *</label
				>
				<select
					id="programArea"
					bind:value={formData.programArea}
					on:change={() => handleInputChange('programArea', formData.programArea)}
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
					>Highest education level achieved *</label
				>
				<select
					id="educationLevel"
					bind:value={formData.educationLevel}
					on:change={() => handleInputChange('educationLevel', formData.educationLevel)}
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
					>Type of Degree *</label
				>
				<select
					id="degreeType"
					bind:value={formData.degreeType}
					on:change={() => handleInputChange('degreeType', formData.degreeType)}
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
					>Graduation Year *</label
				>
				<input
					type="text"
					id="graduationYear"
					bind:value={formData.graduationYear}
					on:input={restrictToNumbers}
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
			<div class="form-group">
				<label for="startTimeframe" class="mb-2 block text-sm font-medium text-gray-700"
					>Desired start timeframe *</label
				>
				<select
					id="startTimeframe"
					bind:value={formData.startTimeframe}
					on:change={() => handleInputChange('startTimeframe', formData.startTimeframe)}
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
				<label for="learningEnvironment" class="mb-2 block text-sm font-medium text-gray-700"
					>Preferred learning environment *</label
				>
				<select
					id="learningEnvironment"
					bind:value={formData.learningEnvironment}
					on:change={() => handleInputChange('learningEnvironment', formData.learningEnvironment)}
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
					>Time to Start *</label
				>
				<select
					id="timeToStart"
					bind:value={formData.timeToStart}
					on:change={() => handleInputChange('timeToStart', formData.timeToStart)}
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
					>Internet Access *</label
				>
				<select
					id="internetAccess"
					bind:value={formData.internetAccess}
					on:change={() => handleInputChange('internetAccess', formData.internetAccess)}
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
			<div class="form-group">
				<label for="ageRange" class="mb-2 block text-sm font-medium text-gray-700"
					>Age Range *</label
				>
				<select
					id="ageRange"
					bind:value={formData.ageRange}
					on:change={() => handleInputChange('ageRange', formData.ageRange)}
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
					>Citizenship *</label
				>
				<select
					id="citizenship"
					bind:value={formData.citizenship}
					on:change={() => handleInputChange('citizenship', formData.citizenship)}
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
					>Military Status *</label
				>
				<select
					id="militaryStatus"
					bind:value={formData.militaryStatus}
					on:change={() => handleInputChange('militaryStatus', formData.militaryStatus)}
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
					<label for="militaryBranch" class="mb-2 block text-sm font-medium text-gray-700"
						>Military Branch *</label
					>
					<select
						id="militaryBranch"
						bind:value={formData.militaryBranch}
						on:change={() => handleInputChange('militaryBranch', formData.militaryBranch)}
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
				<label for="zipCode" class="mb-2 block text-sm font-medium text-gray-700">ZIP Code *</label>
				<input
					type="text"
					id="zipCode"
					bind:value={formData.zipCode}
					on:input={restrictToZipCodeNumbers}
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
			<div class="form-group">
				<label for="firstName" class="mb-2 block text-sm font-medium text-gray-700"
					>First Name *</label
				>
				<input
					type="text"
					id="firstName"
					bind:value={formData.firstName}
					on:input={() => handleInputChange('firstName', formData.firstName)}
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
					>Last Name *</label
				>
				<input
					type="text"
					id="lastName"
					bind:value={formData.lastName}
					on:input={() => handleInputChange('lastName', formData.lastName)}
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
					>Email Address *</label
				>
				<input
					type="email"
					id="email"
					bind:value={formData.email}
					on:input={() => handleInputChange('email', formData.email)}
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
					>Phone Number *</label
				>
				<input
					type="tel"
					id="phone"
					bind:value={formData.phone}
					on:input={restrictToPhoneNumbers}
					on:blur={() => handleBlur('phone')}
					class="w-full rounded-lg border border-gray-300 px-4 py-3 text-gray-700 focus:border-blue-500 focus:ring-2 focus:ring-blue-100 focus:outline-none {errors.phone &&
					touched.phone
						? 'border-red-500'
						: ''}"
					placeholder="e.g., 1234567890"
				/>
				{#if errors.phone && touched.phone}
					<p class="mt-1 text-sm text-red-500">{errors.phone}</p>
				{/if}
			</div>

			<div class="form-group">
				<label for="address" class="mb-2 block text-sm font-medium text-gray-700"
					>Street Address *</label
				>
				<input
					type="text"
					id="address"
					bind:value={formData.address}
					on:input={() => handleInputChange('address', formData.address)}
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
				<label for="city" class="mb-2 block text-sm font-medium text-gray-700">City *</label>
				<input
					type="text"
					id="city"
					bind:value={formData.city}
					on:input={() => handleInputChange('city', formData.city)}
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
				<label for="state" class="mb-2 block text-sm font-medium text-gray-700">State *</label>
				<select
					id="state"
					bind:value={formData.state}
					on:change={() => handleInputChange('state', formData.state)}
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

<style>
	.form-group {
		margin-bottom: 1.5rem;
	}
</style>
