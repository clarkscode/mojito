<script lang="ts">
	import { navLinks } from '../constants';
	import gsap from 'gsap';
	$effect(() => {
		const navTween = gsap.timeline({
			scrollTrigger: {
				trigger: 'nav',
				start: 'bottom top'
			}
		});

		navTween.fromTo(
			'nav',
			{
				backgroundColor: 'transparent'
			},
			{
				backgroundColor: '#00000050',
				backgroundFilter: 'blur(10px)',
				duration: 1,
				ease: 'power1.inOut'
			}
		);
	});
</script>

<nav>
	<div>
		<a href="#home" aria-label="home" class="flex items-center gap-2">
			<img src="/images/logo.png" alt="Mojitos Logo" />
			<p>Negroni</p>
		</a>

		<ul>
			{#each navLinks as link (link.id)}
				<li>
					<a href={`#${link.id}`}>{link.title}</a>
				</li>
			{/each}
		</ul>
	</div>
</nav>
