<script lang="ts">
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	let displayText = '';
	let pressedKeys = new Set<string>();
	let audioContext: AudioContext;
	let audioBuffers: AudioBuffer[] = [];

	// Full 60% Keyboard layout - QWERTY with proper key widths
	const keyboardLayout = [
		[
			{ key: '`', label: '~', width: 1 },
			{ key: '1', label: '1', width: 1 },
			{ key: '2', label: '2', width: 1 },
			{ key: '3', label: '3', width: 1 },
			{ key: '4', label: '4', width: 1 },
			{ key: '5', label: '5', width: 1 },
			{ key: '6', label: '6', width: 1 },
			{ key: '7', label: '7', width: 1 },
			{ key: '8', label: '8', width: 1 },
			{ key: '9', label: '9', width: 1 },
			{ key: '0', label: '0', width: 1 },
			{ key: '-', label: '-', width: 1 },
			{ key: '=', label: '=', width: 1 },
			{ key: 'backspace', label: '⌫', width: 2 }
		],
		[
			{ key: 'tab', label: 'Tab', width: 1.5 },
			{ key: 'q', label: 'Q', width: 1 },
			{ key: 'w', label: 'W', width: 1 },
			{ key: 'e', label: 'E', width: 1 },
			{ key: 'r', label: 'R', width: 1 },
			{ key: 't', label: 'T', width: 1 },
			{ key: 'y', label: 'Y', width: 1 },
			{ key: 'u', label: 'U', width: 1 },
			{ key: 'i', label: 'I', width: 1 },
			{ key: 'o', label: 'O', width: 1 },
			{ key: 'p', label: 'P', width: 1 },
			{ key: '[', label: '[', width: 1 },
			{ key: ']', label: ']', width: 1 },
			{ key: '\\', label: '\\', width: 1.5 }
		],
		[
			{ key: 'capslock', label: 'Caps', width: 1.75 },
			{ key: 'a', label: 'A', width: 1 },
			{ key: 's', label: 'S', width: 1 },
			{ key: 'd', label: 'D', width: 1 },
			{ key: 'f', label: 'F', width: 1 },
			{ key: 'g', label: 'G', width: 1 },
			{ key: 'h', label: 'H', width: 1 },
			{ key: 'j', label: 'J', width: 1 },
			{ key: 'k', label: 'K', width: 1 },
			{ key: 'l', label: 'L', width: 1 },
			{ key: ';', label: ';', width: 1 },
			{ key: "'", label: "'", width: 1 },
			{ key: 'enter', label: '⏎', width: 2.25 }
		],
		[
			{ key: 'shift', label: 'Shift', width: 2.25 },
			{ key: 'z', label: 'Z', width: 1 },
			{ key: 'x', label: 'X', width: 1 },
			{ key: 'c', label: 'C', width: 1 },
			{ key: 'v', label: 'V', width: 1 },
			{ key: 'b', label: 'B', width: 1 },
			{ key: 'n', label: 'N', width: 1 },
			{ key: 'm', label: 'M', width: 1 },
			{ key: ',', label: ',', width: 1 },
			{ key: '.', label: '.', width: 1 },
			{ key: '/', label: '/', width: 1 },
			{ key: 'shift', label: 'Shift', width: 2.75 }
		],
		[
			{ key: 'control', label: 'Ctrl', width: 1.25 },
			{ key: 'meta', label: 'Win', width: 1.25 },
			{ key: 'alt', label: 'Alt', width: 1.25 },
			{ key: ' ', label: '', width: 6.25, isSpace: true },
			{ key: 'alt', label: 'Alt', width: 1.25 },
			{ key: 'meta', label: 'Win', width: 1.25 },
			{ key: 'control', label: 'Menu', width: 1.25 },
			{ key: 'control', label: 'Ctrl', width: 1.25 }
		]
	];

	onMount(async () => {
		// Initialize Web Audio API
		audioContext = new (window.AudioContext || (window as any).webkitAudioContext)();
		
		// Load sound files
		try {
			const soundFiles = ['/assets/sounds/mech1.mp3', '/assets/sounds/mech2.mp3', '/assets/sounds/mech3.mp3'];
			audioBuffers = await Promise.all(soundFiles.map(loadAudio));
		} catch (error) {
			console.warn('Could not load audio files:', error);
		}
	});

	async function loadAudio(url: string): Promise<AudioBuffer> {
		const response = await fetch(url);
		const arrayBuffer = await response.arrayBuffer();
		return await audioContext.decodeAudioData(arrayBuffer);
	}

	function playRandomSound() {
		if (audioBuffers.length === 0) return;
		
		const randomBuffer = audioBuffers[Math.floor(Math.random() * audioBuffers.length)];
		const source = audioContext.createBufferSource();
		source.buffer = randomBuffer;
		source.connect(audioContext.destination);
		source.start();
	}

	function handleKeyDown(event: KeyboardEvent) {
		let key = event.key.toLowerCase();
		
		// Map some keys to match our layout
		if (key === ' ') {
			key = ' ';
			event.preventDefault();
		} else if (key === 'capslock') {
			key = 'capslock';
		} else if (key === 'tab') {
			key = 'tab';
			event.preventDefault();
		} else if (key === 'control') {
			key = 'control';
		} else if (key === 'meta') {
			key = 'meta';
		} else if (key === 'alt') {
			key = 'alt';
		} else if (key === 'shift') {
			key = 'shift';
		}

		// Only handle if not already pressed (prevents key repeat)
		if (pressedKeys.has(key)) return;

		// Add to pressed keys set
		pressedKeys.add(key);
		pressedKeys = pressedKeys; // Trigger reactivity

		// Play sound
		playRandomSound();

		// Handle display text
		if (key === 'enter') {
			if (displayText.toLowerCase() === 'foglomon') {
				goto('/portfolio');
			} else {
				displayText = '';
			}
		} else if (key === 'backspace') {
			displayText = displayText.slice(0, -1);
		} else if (key === ' ') {
			displayText += ' ';
		} else if (key.length === 1 && /^[a-z0-9`\-=\[\];',./\\]$/i.test(key)) {
			displayText += key;
		}
	}

	function handleKeyUp(event: KeyboardEvent) {
		let key = event.key.toLowerCase();
		
		// Map some keys to match our layout
		if (key === ' ') {
			key = ' ';
		} else if (key === 'capslock') {
			key = 'capslock';
		} else if (key === 'tab') {
			key = 'tab';
		} else if (key === 'control') {
			key = 'control';
		} else if (key === 'meta') {
			key = 'meta';
		} else if (key === 'alt') {
			key = 'alt';
		} else if (key === 'shift') {
			key = 'shift';
		}
		
		pressedKeys.delete(key);
		pressedKeys = pressedKeys; // Trigger reactivity
	}
</script>

<svelte:window on:keydown={handleKeyDown} on:keyup={handleKeyUp} />

<div class="scene">
	<div class="table">
		<div class="display-container">
			<div class="eink-display">
				<div class="display-content">
					{#if displayText.length === 0}
						<div class="prompt">Type "foglomon" and press Enter...</div>
					{:else}
						<div class="typed-text">{displayText}</div>
					{/if}
				</div>
			</div>
		</div>
		
		<div class="keyboard-container">
			<div class="keyboard">
				{#each keyboardLayout as row, rowIndex}
					<div class="keyboard-row">
						{#each row as keyData}
							<div 
								class="key" 
								class:pressed={pressedKeys.has(keyData.key)}
								class:space-key={keyData.isSpace}
								style="width: {keyData.width * 48 + (keyData.width - 1) * 6}px;"
							>
								<div class="keycap">
									{keyData.label}
								</div>
							</div>
						{/each}
					</div>
				{/each}
			</div>
		</div>
	</div>
</div>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		overflow: hidden;
		font-family: 'Courier New', monospace;
	}

	.scene {
		width: 100vw;
		height: 100vh;
		background: linear-gradient(135deg, #8B4513 0%, #A0522D 50%, #8B4513 100%);
		background-image: 
			radial-gradient(ellipse at top center, rgba(255, 255, 220, 0.4) 0%, transparent 70%),
			url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23654321' fill-opacity='0.1'%3E%3Cpath d='M0 0h60v60H0z'/%3E%3Cpath d='M30 30h30v30H30z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
		display: flex;
		justify-content: center;
		align-items: center;
		perspective: 1000px;
		position: relative;
	}

	.scene::before {
		content: '';
		position: absolute;
		top: 0;
		left: 50%;
		transform: translateX(-50%);
		width: 80%;
		height: 60%;
		background: radial-gradient(ellipse at center, rgba(255, 255, 220, 0.3) 0%, transparent 70%);
		pointer-events: none;
		z-index: 1;
	}

	.table {
		width: 85%;
		max-width: 1400px;
		height: 80%;
		position: relative;
		transform: rotateX(5deg);
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 80px;
	}

	.display-container {
		position: relative;
		z-index: 10;
	}

	.eink-display {
		width: 400px;
		height: 240px;
		background: #f0f0f0;
		border: 8px solid #2a2a2a;
		border-radius: 8px;
		position: relative;
		box-shadow: 
			0 10px 30px rgba(0, 0, 0, 0.3),
			inset 0 2px 5px rgba(255, 255, 255, 0.2);
	}

	.eink-display::before {
		content: '';
		position: absolute;
		top: -15px;
		right: 10px;
		width: 20px;
		height: 10px;
		background: #444;
		border-radius: 2px;
	}

	.display-content {
		padding: 20px;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		box-sizing: border-box;
	}

	.prompt {
		color: #666;
		font-size: 18px;
		text-align: center;
		animation: pulse 2s infinite;
	}

	.typed-text {
		color: #000;
		font-size: 24px;
		font-weight: bold;
		text-align: center;
		word-break: break-all;
		position: relative;
	}

	.typed-text::after {
		content: '|';
		animation: blink 1s infinite;
		margin-left: 2px;
	}

	@keyframes blink {
		0%, 50% { opacity: 1; }
		51%, 100% { opacity: 0; }
	}

	@keyframes pulse {
		0%, 100% { opacity: 1; }
		50% { opacity: 0.5; }
	}

	.keyboard-container {
		width: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.keyboard {
		background: #1a1a1a;
		padding: 30px 40px; /* Increased padding for larger keys */
		border-radius: 12px;
		box-shadow: 
			0 25px 50px rgba(0, 0, 0, 0.6),
			inset 0 3px 8px rgba(255, 255, 255, 0.1),
			inset 0 -3px 8px rgba(0, 0, 0, 0.3);
		border: 3px solid #333;
		width: fit-content;
		position: relative;
	}

	.keyboard::before {
		content: '';
		position: absolute;
		top: -5px;
		left: -5px;
		right: -5px;
		bottom: -5px;
		background: linear-gradient(45deg, #2a2a2a, #1a1a1a);
		border-radius: 15px;
		z-index: -1;
	}

	.keyboard-row {
		display: flex;
		justify-content: center;
		margin-bottom: 8px;
		gap: 6px;
	}

	.keyboard-row:last-child {
		margin-bottom: 0;
	}

	.key {
		position: relative;
		transition: all 0.1s ease;
		height: 48px;
		/* Width is now set dynamically in the template */
	}

	.keycap {
		width: 100%;
		height: 48px;
		background: linear-gradient(145deg, #DAA520, #B8860B);
		border: 2px solid #8B7355;
		border-radius: 6px;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 11px; /* Smaller font for modifier keys */
		font-weight: bold;
		color: #2a2a2a;
		box-shadow: 
			0 5px 10px rgba(0, 0, 0, 0.4),
			inset 0 2px 4px rgba(255, 255, 255, 0.3),
			inset 0 -2px 4px rgba(0, 0, 0, 0.2);
		cursor: pointer;
		transition: all 0.1s ease;
		box-sizing: border-box;
		position: relative;
	}

	.keycap::before {
		content: '';
		position: absolute;
		top: 1px;
		left: 1px;
		right: 1px;
		bottom: 1px;
		background: linear-gradient(145deg, rgba(255,255,255,0.1), transparent);
		border-radius: 4px;
		pointer-events: none;
	}

	.space-key .keycap {
		/* Space key styling handled by parent .key.space-key width */
	}

	.key.pressed .keycap {
		transform: translateY(2px);
		box-shadow: 
			0 2px 4px rgba(0, 0, 0, 0.3),
			inset 0 1px 2px rgba(255, 255, 255, 0.3),
			inset 0 -1px 2px rgba(0, 0, 0, 0.2);
		background: linear-gradient(145deg, #B8860B, #DAA520);
	}

	.keycap:hover {
		transform: translateY(1px);
	}

	/* Mobile responsive adjustments */
	@media (max-width: 768px) {
		.keyboard {
			padding: 20px 25px;
			transform: scale(0.75);
		}
		
		.eink-display {
			width: 320px;
			height: 192px;
		}
		
		.table {
			width: 95%;
			gap: 60px;
		}
		
		.data-pins {
			display: none; /* Hide pins on very small screens */
		}
	}

	@media (max-width: 480px) {
		.keyboard {
			transform: scale(0.6);
			padding: 15px 20px;
		}
		
		.eink-display {
			width: 280px;
			height: 168px;
		}

		.table {
			gap: 40px;
		}
	}
</style>
