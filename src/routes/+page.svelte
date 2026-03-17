<script lang="ts">
	const synonyms = [
		'Electronic noise experimentation',
		'Sonic chaos exploration',
		'Digital sound alchemy',
		'Audio glitch laboratory',
		'Synthetic noise crafting',
		'Circuit-bent soundscapes',
		'Lo-fi signal processing',
		'Voltage-controlled madness',
		'Experimental audio synthesis'
	];

	const socialLinks = [
		{
			name: 'Instagram',
			href: 'https://www.instagram.com/digim.i/',
			icon: `<rect x="2" y="2" width="20" height="20" rx="5" ry="5"></rect><path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"></path><line x1="17.5" x2="17.51" y1="6.5" y2="6.5"></line>`
		},
		{
			name: 'YouTube',
			href: 'https://www.youtube.com/@digim.i-iy7oj',
			icon: `<path d="M22.54 6.42a2.78 2.78 0 0 0-1.94-2C18.88 4 12 4 12 4s-6.88 0-8.6.46a2.78 2.78 0 0 0-1.94 2A29 29 0 0 0 1 11.75a29 29 0 0 0 .46 5.33A2.78 2.78 0 0 0 3.4 19c1.72.46 8.6.46 8.6.46s6.88 0 8.6-.46a2.78 2.78 0 0 0 1.94-2 29 29 0 0 0 .46-5.25 29 29 0 0 0-.46-5.33z"></path><polygon points="9.75 15.02 15.5 11.75 9.75 8.48 9.75 15.02"></polygon>`
		},
		{
			name: 'Threads',
			href: 'https://www.threads.com/@digim.i',
			icon: `<circle cx="12" cy="12" r="10"></circle><path d="M20.4 12c0 4.7-3.8 8.4-8.4 8.4-4.7 0-8.4-3.8-8.4-8.4"></path><path d="M12 7c-2.8 0-5 2.2-5 5s2.2 5 5 5 5-2.2 5-5-2.2-5-5-5z"></path>`
		}
	];

	let currentIndex = $state(Math.floor(Math.random() * synonyms.length));

	function rotateText() {
		let newIndex;
		do {
			newIndex = Math.floor(Math.random() * synonyms.length);
		} while (newIndex === currentIndex && synonyms.length > 1);
		currentIndex = newIndex;
	}
</script>

<svelte:head>
	<title>Digim.i</title>
</svelte:head>

<div class="min-h-screen flex flex-col items-center justify-center p-8" style="background: radial-gradient(circle at center, #3d4d5d 0%, #2d3d4d 40%, #1d2d3d 100%);">
	<h1 class="text-6xl md:text-8xl font-bold text-white mb-8 tracking-tight">
		Digim.i
	</h1>

	<button
		onclick={rotateText}
		class="text-2xl md:text-3xl text-white/90 hover:text-white transition-all duration-300 cursor-pointer mb-16 font-light"
	>
		{synonyms[currentIndex]}
	</button>

	<div class="flex gap-8">
		{#each socialLinks as link}
			<a
				href={link.href}
				target="_blank"
				rel="noopener noreferrer"
				class="text-white/70 hover:text-white transition-all duration-300 hover:scale-110"
				aria-label={link.name}
			>
				<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
					{@html link.icon}
				</svg>
			</a>
		{/each}
	</div>
</div>
