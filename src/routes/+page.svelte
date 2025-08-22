<script lang="ts">
	import Navbar from '../components/Navbar.svelte';
	import Hero from '../components/Hero.svelte';
	import gsap from 'gsap';
	import { ScrollTrigger, SplitText } from 'gsap/all';

	$effect(() => {
		gsap.registerPlugin(ScrollTrigger, SplitText);
	});
</script>

<main>
	<Navbar />
	<Hero />
	<div class="h-dvh bg-black"></div>
</main>
