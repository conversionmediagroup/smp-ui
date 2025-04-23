<script lang="ts">
	import { isMobileMenuOpen } from '$lib/stores';

	import { cn } from '$lib/ultils';

	export let menuButton: HTMLDivElement | undefined = undefined;

	function toggleMenu() {
		isMobileMenuOpen.update((value) => !value);
		// $isMobileMenuOpen = true
	}
</script>

<div class="lg:hidden" bind:this={menuButton}>
	<button
		class="relative z-100 scale-120 border-none p-0 hover:bg-none focus:bg-none"
		onclick={toggleMenu}
		aria-label="Mobile Menu button"
	>
		<div
			class={cn(
				'ring-opacity-50 relative flex w-[40px] transform items-center justify-center overflow-hidden  rounded-full ring-0 ring-gray-500 transition-all duration-200'
			)}
		>
			<div
				class="flex h-[15px] w-[20px] origin-center transform flex-col items-center justify-evenly overflow-hidden transition-all duration-300"
			>
				<div
					class={cn('h-[1px] w-4 origin-left transform bg-white transition-all duration-300', {
						'translate-x-10': $isMobileMenuOpen
					})}
				></div>
				<div
					class={cn(
						'h-[1px] w-4 origin-left transform bg-white transition-all delay-75 duration-300',
						{
							'translate-x-10': $isMobileMenuOpen
						}
					)}
				></div>
				<div
					class={cn(
						'h-[1px] w-4 origin-left transform bg-white transition-all delay-150 duration-300',
						{
							'translate-x-10': $isMobileMenuOpen
						}
					)}
				></div>
				<div
					class={cn(
						'absolute top-2 flex w-0 -translate-x-10 transform items-center justify-between transition-all duration-500',
						{
							'w-5 translate-x-0': $isMobileMenuOpen
						}
					)}
				>
					<div
						class={cn(
							'absolute h-[1px] w-5 transform bg-white transition-all delay-300 duration-500',
							{
								'rotate-45': $isMobileMenuOpen
							}
						)}
					></div>
					<div
						class={cn(
							'absolute h-[1px] w-5 transform bg-white transition-all delay-300 duration-500',
							{
								'-rotate-45': $isMobileMenuOpen
							}
						)}
					></div>
				</div>
			</div>
		</div>
	</button>
</div>
