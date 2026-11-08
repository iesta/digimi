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

	const PARAMS: Record<string, { label: string; min: number; max: number }> = {
		scan: { label: 'SCAN', min: 0, max: 60 },
		grain: { label: 'GRAIN', min: 0, max: 80 },
		sat: { label: 'SAT', min: 0, max: 300 },
		dim: { label: 'DIM', min: 0, max: 90 }
	};

	const PRESETS: Record<string, Record<string, number>> = {
		none: { dim: 65 },
		vhs: { scan: 25, sat: 150, dim: 65 },
		grain: { grain: 40, sat: 120, dim: 60 },
		mono: { sat: 0, dim: 60 },
		glitch: { scan: 15, sat: 200, dim: 60 }
	};

	let preset = $state('vhs');
	let vals = $state<Record<string, number>>({ ...PRESETS.vhs });

	const CLIPS = ['/bg-2.mp4', '/bg.mp4', '/bg-3.mp4', '/bg-4.mp4', '/bg-5.mp4', '/bg-6.mp4', '/bg-7.mp4'];
	let clip = $state(0);

	let soundOn = $state(false);
	let volume = $state(2);
	let playing = $state(true);
	let videoEl = $state<HTMLVideoElement | null>(null);
	let fadeId = 0;

	function targetVolume() {
		return soundOn ? volume / 6 : 0;
	}

	function fadeTo(target: number, ms = 220, done?: () => void) {
		const v = videoEl;
		if (!v) return done?.();
		const from = v.volume;
		if (Math.abs(from - target) < 0.01) {
			v.volume = target;
			return done?.();
		}
		const id = ++fadeId;
		const start = performance.now();
		const step = (now: number) => {
			if (id !== fadeId || videoEl !== v) return;
			const k = Math.min(1, (now - start) / ms);
			v.volume = from + (target - from) * k;
			if (k < 1) requestAnimationFrame(step);
			else done?.();
		};
		requestAnimationFrame(step);
	}

	async function nextClip() {
		fadeTo(0, 250);
		await new Promise((r) => setTimeout(r, 500));
		playing = true;
		clip = (clip + 1) % CLIPS.length;
	}

	function syncAudio() {
		if (!videoEl) return;
		const v = videoEl;
		v.playbackRate = 1.125; // 0.9× effective over 0.8× baked clips
		v.muted = !soundOn || volume === 0;
		v.volume = 0;
		if (!v.muted) fadeTo(targetVolume(), 700);
	}

	function toggleSound() {
		soundOn = !soundOn;
		if (!videoEl) return;
		if (soundOn) {
			videoEl.muted = false;
			videoEl.volume = 0;
			videoEl.play().catch(() => {});
			fadeTo(targetVolume());
		} else {
			fadeTo(0, 220, () => {
				if (videoEl) videoEl.muted = true;
			});
		}
	}

	function setVolume(level: number) {
		volume = volume === level ? 0 : level;
		soundOn = true;
		if (videoEl) videoEl.muted = volume === 0;
		fadeTo(targetVolume());
	}

	function togglePlay() {
		if (!videoEl) return;
		if (videoEl.paused) {
			playing = true;
			videoEl.play().catch(() => {});
			fadeTo(targetVolume());
		} else {
			playing = false;
			fadeTo(0, 220, () => {
				if (videoEl) {
					videoEl.pause();
					videoEl.volume = 0;
				}
			});
		}
	}

	function applyPreset() {
		vals = { ...PRESETS[preset] };
	}

	function randomize() {
		const keys = Object.keys(PRESETS);
		preset = keys[Math.floor(Math.random() * keys.length)];
		const next: Record<string, number> = {};
		for (const id of Object.keys(PRESETS[preset])) {
			const { min, max } = PARAMS[id];
			next[id] = Math.round(min + Math.random() * (max - min));
		}
		vals = next;
	}

	function handleKey(event: KeyboardEvent) {
		if (event.code !== 'Space') return;
		const el = document.activeElement as HTMLElement | null;
		const tag = el?.tagName;
		if (tag === 'INPUT' || tag === 'SELECT' || tag === 'BUTTON' || tag === 'TEXTAREA') return;
		event.preventDefault();
		togglePlay();
	}

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

<svelte:window onkeydown={handleKey} />

<div 
	class="relative min-h-screen overflow-hidden bg-[#1d2d3d] flex flex-col items-center justify-center p-8"
	style="--scan: {vals.scan ?? 0}; --grain: {vals.grain ?? 0}; --sat: {vals.sat ?? 100}; --dim: {vals.dim ?? 65};"
	onmousemove={handleMouseMove}
>
	{#key clip}
		<video
			src={CLIPS[clip]}
			bind:this={videoEl}
			muted
			oncanplay={syncAudio}
			class="absolute inset-0 h-full w-full object-cover scale-105 motion-reduce:hidden {preset === 'glitch' ? '[animation:glitch_6s_steps(1)_infinite,clip-in_600ms_ease-out]' : '[animation:clip-in_600ms_ease-out]'}"
			style="filter: contrast(1.1) brightness(0.9) saturate(calc(var(--sat) * 1%)) sepia(0.12)"
			autoplay
			playsinline
			aria-hidden="true"
			onended={nextClip}
		></video>
	{/key}

	<div
		class="absolute inset-0 pointer-events-none bg-[repeating-linear-gradient(to_bottom,rgba(0,0,0,0.6)_0px,rgba(0,0,0,0.6)_1px,transparent_1px,transparent_3px)]"
		style="opacity: calc(var(--scan) / 100)"
	></div>

	<div
		class="vhs-grain absolute inset-0 pointer-events-none mix-blend-overlay"
		style="opacity: calc(var(--grain) / 100)"
	></div>

	<div class="absolute inset-x-0 top-0 h-24 pointer-events-none bg-gradient-to-b from-transparent via-white/10 to-transparent motion-reduce:animate-none [animation:vhs-band_7s_linear_infinite]"></div>

	<div class="absolute inset-0 pointer-events-none bg-[radial-gradient(ellipse_at_center,transparent_40%,rgba(0,0,0,0.8)_100%)]"></div>

	<div
		class="absolute inset-0 pointer-events-none"
		style="opacity: calc(var(--dim) / 100); background: radial-gradient(circle at {mouseX}% {mouseY}%, #3d4d5d 0%, #2d3d4d 40%, #1d2d3d 100%);"
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

			<button
				type="button"
				onclick={togglePlay}
				class="cursor-pointer text-white/70 hover:text-white transition-all duration-300 hover:scale-110"
				aria-label={playing ? 'Pause background video' : 'Play background video'}
				aria-pressed={playing}
			>
				<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
					{#if playing}
						<line x1="9" y1="5" x2="9" y2="19" />
						<line x1="15" y1="5" x2="15" y2="19" />
					{:else}
						<path d="M7 4.5 19 12 7 19.5z" />
					{/if}
				</svg>
			</button>

			<button
				type="button"
				onclick={toggleSound}
				class="cursor-pointer text-white/70 hover:text-white transition-all duration-300 hover:scale-110"
				aria-label={soundOn ? 'Mute background video' : 'Unmute background video'}
				aria-pressed={soundOn}
			>
				<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
					<path d="M11 5 6 9H3v6h3l5 4z" />
					{#if soundOn}
						<path d="M15.5 8.5a5 5 0 0 1 0 7" />
						<path d="M18.5 5.5a9 9 0 0 1 0 13" />
					{:else}
						<line x1="16" y1="9" x2="21" y2="15" />
						<line x1="21" y1="9" x2="16" y2="15" />
					{/if}
				</svg>
			</button>
		</div>

		<div class="mt-8 flex items-center gap-2">
			{#each CLIPS as c, i (c)}
				<button
					type="button"
					onclick={() => {
						clip = i;
						soundOn = true;
					}}
					class="cursor-pointer rounded-full transition-all duration-200 {i === clip ? 'size-[18px] bg-white/80' : 'size-3 bg-white/25 hover:bg-white/50'}"
					aria-label={`Play clip ${i + 1}`}
					aria-current={i === clip}
				></button>
			{/each}
		</div>

		{#if soundOn}
			<div class="mt-6 flex h-9 items-end gap-1 rounded-full bg-black/30 px-2 py-0.5">
				{#each [1, 2, 3, 4, 5, 6] as level}
					<button
						type="button"
						onclick={() => setVolume(level)}
						class="w-[7.5px] cursor-pointer rounded-sm transition-colors {level <= volume ? 'bg-white/90' : 'bg-white/30 hover:bg-white/50'}"
						style="height: {level * 5}px"
						aria-label={`Volume ${level} of 6`}
						aria-current={level <= volume}
					></button>
				{/each}
			</div>
		{:else}
			<div class="mt-6 h-9"></div>
		{/if}
	</div>

	<div
		class="absolute bottom-6 right-6 z-10 flex w-52 flex-col gap-2 rounded-xl border border-white/10 bg-black/25 px-4 py-3 font-mono text-[10px] tracking-[0.2em] text-white/50 backdrop-blur-md select-none"
	>
		<div class="flex items-center gap-2">
			<select
				bind:value={preset}
				onchange={applyPreset}
				class="w-full cursor-pointer rounded border border-white/10 bg-black/30 px-2 py-1 uppercase tracking-[0.2em] text-white/70 outline-none"
				aria-label="Effect preset"
			>
				{#each Object.keys(PRESETS) as p}
					<option value={p} class="bg-neutral-900">{p}</option>
				{/each}
			</select>

			<button
				type="button"
				onclick={randomize}
				class="grid size-7 shrink-0 cursor-pointer place-items-center rounded border border-white/10 text-white/60 transition-colors hover:border-white/25 hover:text-white"
				aria-label="Randomize effect"
				title="Randomize"
			>
				<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
					<rect x="3" y="3" width="18" height="18" rx="3" />
					<circle cx="8" cy="8" r="1.2" fill="currentColor" />
					<circle cx="16" cy="8" r="1.2" fill="currentColor" />
					<circle cx="12" cy="12" r="1.2" fill="currentColor" />
					<circle cx="8" cy="16" r="1.2" fill="currentColor" />
					<circle cx="16" cy="16" r="1.2" fill="currentColor" />
				</svg>
			</button>
		</div>

		{#each Object.keys(vals) as id}
			<label class="flex items-center justify-between gap-3">
				{PARAMS[id].label}
				<input
					type="range"
					min={PARAMS[id].min}
					max={PARAMS[id].max}
					bind:value={vals[id]}
					class="h-1 w-24 cursor-pointer accent-white/70"
				/>
			</label>
		{/each}
	</div>
</div>