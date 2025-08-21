<script lang="ts">
	import gsap from 'gsap';
	import { ScrollTrigger, SplitText } from 'gsap/all';
	import { onDestroy, untrack } from 'svelte';

	let videoRef: HTMLVideoElement;

	let isMobile = $state(false);

	$effect(() => {
		const mediaQuery = window.matchMedia('(max-width: 767px)');
		isMobile = mediaQuery.matches;
		const listener = () => {
			isMobile = mediaQuery.matches;
		};
		mediaQuery.addEventListener('change', listener);

		return () => {
			mediaQuery.removeEventListener('change', listener);
		};
	});

	$effect(() => {
		gsap.registerPlugin(ScrollTrigger, SplitText);

		// Create an async function to wait for fonts
		const initAnimations = async () => {
			// Wait for fonts to be ready to prevent SplitText layout shifts
			await document.fonts.ready;

			const context = gsap.context(() => {
				const heroSplit = new SplitText('.title', { type: 'chars, words' });
				const paragraphSplit = new SplitText('.subtitle', { type: 'lines' });

				heroSplit.chars.forEach((char) => {
					char.classList.add('text-gradient');
				});

				gsap.from(heroSplit.chars, {
					yPercent: 100,
					duration: 1.8,
					ease: 'expo.out',
					stagger: 0.05
				});

				gsap.from(paragraphSplit.lines, {
					opacity: 0,
					yPercent: 100,
					duration: 1.8,
					ease: 'expo.out',
					stagger: 0.05,
					delay: 1
				});

				gsap
					.timeline({
						scrollTrigger: {
							trigger: '#hero',
							start: 'top top',
							end: 'bottom top',
							scrub: true
						}
					})
					.to('.right-leaf', { y: 200 }, 0)
					.to('.left-leaf', { y: -200 }, 0);

				// 2. Wrap reads of `isMobile` in untracked()
				const startValue = untrack(() => (isMobile ? 'top 50%' : 'center 60%'));
				const endValue = untrack(() => (isMobile ? '120% top' : 'bottom top'));

				const videoTimeLine = gsap.timeline({
					scrollTrigger: {
						trigger: videoRef,
						start: startValue,
						end: endValue,
						scrub: true,
						pin: true
					}
				});

				if (videoRef) {
					const setupVideoAnimation = () => {
						videoTimeLine.to(videoRef, {
							currentTime: videoRef.duration
						});
					};

					// 3. Add the readyState check for the video
					if (videoRef.readyState >= videoRef.HAVE_METADATA) {
						setupVideoAnimation();
					} else {
						videoRef.onloadedmetadata = setupVideoAnimation;
					}
				}
			}); // End of context

			// Return the context so we can clean it up
			return context;
		};

		let ctx: gsap.Context;
		initAnimations().then((context) => {
			ctx = context;
		});

		onDestroy(() => {
			ctx && ctx.revert();
		});
	});
</script>

<div>
	<section id="hero" class="noisy">
		<h1 class="title uppercase">Mojito</h1>
		<img src="/images/hero-left-leaf.png" alt="left leaf" class="left-leaf" />
		<img src="/images/hero-right-leaf.png" alt="right leaf" class="right-leaf" />

		<div class="body">
			<div class="content">
				<div class="space-y-5 hidden md:block">
					<p>Cool. Crisp. Classic</p>
					<p class="subtitle">
						Sip The Spirit
						<br />
						of Summer
					</p>
				</div>
				<div class="view-cocktails">
					<p class="subtitle">
						Every cocktail on our menu is a blend of premium ingredients, creative flair, and
						timeless recipes — designed to delight your senses.
					</p>
					<a href="#cocktails" aria-label="view cocktails">View Cocktails</a>
				</div>
			</div>
		</div>
	</section>
	<div class="video absolute inset-0">
		<video bind:this={videoRef} muted playsInline preload="auto" src="/videos/output.mp4"></video>
	</div>
</div>
