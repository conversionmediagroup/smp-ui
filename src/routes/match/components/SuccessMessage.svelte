<script lang="ts">
	import { onMount } from 'svelte';
	import { scale } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
	import { Check, GraduationCap, BookOpen, Globe } from 'lucide-svelte';
	import confetti from 'canvas-confetti';

	onMount(() => {
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
	});
</script>

<div
	in:scale={{ duration: 400, delay: 200, easing: quintOut }}
	class="relative rounded-2xl border border-green-100 bg-white p-8 text-center shadow-sm"
>
	<canvas id="confetti-canvas" class="pointer-events-none absolute inset-0 h-full w-full"></canvas>

	<div
		class="relative z-10 mx-auto mb-4 flex h-16 w-16 items-center justify-center rounded-full bg-green-100"
	>
		<Check size={32} class="text-green-600" />
	</div>
	<h3 class="relative z-10 mb-2 text-2xl font-bold text-gray-900">Congratulations!</h3>
	<p class="relative z-10 mx-auto mb-6 max-w-2xl text-gray-600">
		Thank you for completing your profile. We've found <span class="font-semibold text-blue-600"
			>3 schools</span
		> that match your preferences! You'll receive an email with your personalized recommendations within
		24 hours.
	</p>

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
