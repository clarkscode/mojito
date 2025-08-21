<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import '../app.css';
	let { children } = $props();
	import { onMount } from 'svelte';
	import { SplitText, ScrollTrigger } from 'gsap/all';
	import gsap from 'gsap';
	onMount(() => {
		gsap.registerPlugin(ScrollTrigger, SplitText);
	});
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children?.()}
