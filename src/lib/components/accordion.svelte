<script lang="ts">
	/** Accordion component for displaying collapsible content */

	export let open = false;
	import { slide } from 'svelte/transition';

	let chevron: HTMLImageElement;
	const handleClick = () => {
		chevron.style.transform = open ? 'rotate(0deg)' : 'rotate(90deg)';
		open = !open;
	};
</script>

<div class="bg-[#C6D1EF] border border-white rounded-lg">
	<button
		class="w-full py-6 px-4 transition-bg-color rounded-lg bg-body-background-white hover:bg-colorstackuf-orange duration-300"
		on:click={handleClick}
	>
		<div class="flex w-full h-full items-center">
			<div class="flex-[1] mr-[10px] text-left">
				<slot name="head"></slot>
			</div>

			<img
				src="/chevron.svg"
				alt="chevron"
				bind:this={chevron}
				class="transition linear duration-300 w-[20px] h-[20px]"
			/>
		</div>
	</button>

	<!-- Content to display-->
	{#if open}
		<div class="p-4" transition:slide>
			<slot name="details"></slot>
		</div>
	{/if}
</div>
