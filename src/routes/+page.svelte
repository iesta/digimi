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
		'Experimental audio synthesis',
		'Oscillator worship rituals',
		'Tape hiss and feedback loops',
		'Modular drone transmissions',
		'Cathode ray hypnosis session',
		'Broken sequencer poetry',
		'Sine wave exorcism',
		'Grain, dust and overload',
		'Square wave tantrums at 3am',
		'Reverb-soaked breakdown',
		'Filter sweep to nowhere',
		'Noise floor archaeology',
		'Patch cables in a blender',
		'Self-oscillating lullaby'
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
	let mouseX = $state(50);
	let mouseY = $state(50);

	let scan = $state(25);
	let sat = $state(150);
	let dim = $state(65);

	function handleMouseMove(event: MouseEvent) {
		mouseX = (event.clientX / window.innerWidth) * 100;
		mouseY = (event.clientY / window.innerHeight) * 100;
	}

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

<div 
	class="relative min-h-screen overflow-hidden bg-[#1d2d3d] flex flex-col items-center justify-center p-8"
	onmousemove={handleMouseMove}
>
	<video
		class="absolute inset-0 h-full w-full object-cover scale-105 motion-reduce:hidden"
		style="filter: contrast(1.1) brightness(0.9) saturate({sat}%) sepia(0.12)"
		autoplay
		muted
		loop
		playsinline
		aria-hidden="true"
	>
		<source src="/bg.mp4" type="video/mp4" />
	</video>

	<div
		class="absolute inset-0 pointer-events-none bg-[repeating-linear-gradient(to_bottom,rgba(0,0,0,0.6)_0px,rgba(0,0,0,0.6)_1px,transparent_1px,transparent_3px)]"
		style="opacity: {scan / 100}"
	></div>

	<div class="absolute inset-x-0 top-0 h-24 pointer-events-none bg-gradient-to-b from-transparent via-white/10 to-transparent motion-reduce:animate-none [animation:vhs-band_7s_linear_infinite]"></div>

	<div class="absolute inset-0 pointer-events-none bg-[radial-gradient(ellipse_at_center,transparent_40%,rgba(0,0,0,0.8)_100%)]"></div>

	<div
		class="absolute inset-0 pointer-events-none"
		style="opacity: {dim / 100}; background: radial-gradient(circle at {mouseX}% {mouseY}%, #3d4d5d 0%, #2d3d4d 40%, #1d2d3d 100%);"
	></div>

	<div class="relative flex flex-col items-center [text-shadow:0_2px_24px_rgba(0,0,0,0.85)]">
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

	<div
		class="absolute bottom-6 right-6 z-10 flex flex-col gap-2 rounded-xl border border-white/10 bg-black/25 px-4 py-3 font-mono text-[10px] tracking-[0.2em] text-white/50 backdrop-blur-md select-none"
	>
		<label class="flex items-center justify-between gap-3">
			SCAN
			<input type="range" min="0" max="60" bind:value={scan} class="h-1 w-20 accent-white/70" />
		</label>
		<label class="flex items-center justify-between gap-3">
			SAT
			<input type="range" min="50" max="300" bind:value={sat} class="h-1 w-20 accent-white/70" />
		</label>
		<label class="flex items-center justify-between gap-3">
			DIM
			<input type="range" min="0" max="90" bind:value={dim} class="h-1 w-20 accent-white/70" />
		</label>
	</div>
</div>