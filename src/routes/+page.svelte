<script lang="ts">
	import Header from './components/Header/Header.svelte';
	import PartnerScrollers from './components/Partners/PartnerScrollers.svelte';
	import Hero from './components/Hero/Hero.svelte';
	import Footer from './components/Footer/Footer.svelte';
	import Credits from './components/Partners/Credits.svelte';
	import FAQs from './components/FAQ/FAQs.svelte';
</script>

<div class="flex flex-col">
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
