<script lang="ts">
	import { slide } from 'svelte/transition';
	import { Hamburger } from 'svelte-hamburgers';
	import { page } from '$app/stores';
	import { onMount } from 'svelte';
	import { onAbout, onSponsors, onStudents } from '$lib/currentPage';

	export let ariaLabel: string = 'Toggle navigation';

	let open: boolean = false;

	function toggleMenu() {
		open = !open;
		document.body.classList.toggle('no-scroll', open);
	}

	function toggleBurger() {
		document.body.classList.toggle('no-scroll', !open);
	}

	$onAbout = $page.url.pathname === '/about';
	$onSponsors = $page.url.pathname === '/sponsors';
	$onStudents = $page.url.pathname === '/students';

	const handleStyleChange = (page: string) => {
		$onAbout = page === 'about';
		$onSponsors = page === 'sponsors';
		$onStudents = page === 'students';
	};

	$: innerWidth = 0;
	$: {
		// Close the menu when the screen is resized to desktop
		if (innerWidth >= 1024) {
			open = false;
			document.body.classList.toggle('no-scroll', false);
		}
	}

	onMount(() => {
		return () => {
			document.body.classList.remove('no-scroll');
		};
	});
</script>

<svelte:window bind:innerWidth />
<div
	class="h-[80px] px-mobile-padding-x tablet:px-tablet-padding-x laptop:px-laptop-padding-x h-[5rem] flex flex-col items-center justify-center font-gotham-book mt-4"
>
	<div class="w-full max-w-page-width h-full flex items-center">
		<!-- Logo and Title -->
		<a
			href="/"
			class="flex-[1] pt-1"
			on:click={() => {
				open = false;
				handleStyleChange('');
				document.body.classList.toggle('no-scroll', false);
			}}
		>
			<img
				class="object-contain w-[20rem]"
				src="/logos/colorstack-logo-title.svg"
				alt="Colorstack logo and title"
			/>
		</a>

		<!-- Navigation Section -->
		<div class="flex items-center gap-12 h-full">
			{#if innerWidth >= 1024}
				<a
					on:click={() => handleStyleChange('about')}
					class:on-page={$onAbout}
					class="nav-btn text-white hover:text-colorstackuf-orange transition-colors duration-300 pt-1"
					href="/about">About</a
				>
				<a
					on:click={() => handleStyleChange('sponsors')}
					class:on-page={$onSponsors}
					class="nav-btn text-white hover:text-colorstackuf-orange transition-colors duration-300 pt-1"
					href="/sponsors">Sponsors</a
				>
				<a
					on:click={() => handleStyleChange('students')}
					class:on-page={$onStudents}
					class="nav-btn text-white hover:text-colorstackuf-orange transition-colors duration-300 pt-1"
					href="/students">Students</a
				>
				<a
					class="border-2 border-solid border-white self-center flex items-center pt-4 pb-[1.1rem] px-6 rounded-[1.4rem] hover:bg-colorstackuf-blue hover:border-colorstackuf-blue text-white hover:text-black transition-colors duration-300"
					href="https://linktr.ee/colorstackuf"
					target="_blank"
				>
					<p class="translate-y-[0.5rem] text-[1.2rem] no-underline text-align-center">
						Become a Member
					</p>
				</a>
			{:else}
				<!-- Hamburger Menu -->
				<div>
					{#if open}
						<div class="absolute top-[86px] left-0 w-full h-full bg-black bg-opacity-90 z-20" />
						<div
							transition:slide
							class="absolute w-full left-0 top-[86px] h-[14rem] flex flex-col items-left justify-evenly bg-body-background-blue px-6 py-8 gap-4 z-20"
						>
							<a
								class="text-white hover:text-colorstackuf-orange transition-colors duration-300 text-xl pt-1"
								class:on-page={$onAbout}
								href="/about"
								on:click={() => {
									handleStyleChange('about');
									toggleMenu();
								}}>About</a
							>
							<a
								class="text-white hover:text-colorstackuf-orange transition-colors duration-300 text-xl pt-1"
								href="/sponsors"
								class:on-page={$onSponsors}
								on:click={() => {
									handleStyleChange('sponsors');
									toggleMenu();
								}}>Sponsors</a
							>
							<a
								class="text-white hover:text-colorstackuf-orange transition-colors duration-300 text-xl pt-1"
								href="/students"
								class:on-page={$onStudents}
								on:click={() => {
									handleStyleChange('students');
									toggleMenu();
								}}>Students</a
							>
							<a
								class="text-white hover:text-colorstackuf-orange transition-colors duration-300 text-xl pt-1"
								href="https://linktr.ee/colorstackuf"
								target="_blank">Get Involved</a
							>
						</div>
					{/if}

					<Hamburger
						--color={'#fd9739'}
						{ariaLabel}
						type="slider"
						on:click={toggleBurger}
						bind:open
					/>
				</div>
			{/if}
		</div>
	</div>
</div>

<style>
	.nav-btn {
		transform: translateY(0.5rem);
		font-size: 1.2rem;
		text-decoration: none;
	}
</style>
