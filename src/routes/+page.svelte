<script lang="ts">
	import gsap from 'gsap';
	import { ScrollTrigger, SplitText } from 'gsap/all';
	import Navbar from '../components/Navbar.svelte';
	import Hero from '../components/Hero.svelte';

	$effect(() => {
		gsap.registerPlugin(ScrollTrigger, SplitText);
	});
</script>

<main>
	<Navbar />
	<Hero />
	<div class="h-dvh bg-black"></div>
</main>
