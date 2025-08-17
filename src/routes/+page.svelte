<script lang="ts">
	import gsap from 'gsap';
	import { ScrollTrigger, SplitText } from 'gsap/all';

	$effect(() => {
		gsap.registerPlugin(ScrollTrigger, SplitText);
	});
</script>

<div class="h-[100vh] flex-center">
	<h1 class="text-3xl font-bold">Hello this is hero</h1>
</div>
