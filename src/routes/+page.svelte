<script lang="ts">
	import {onMount, onDestroy} from "svelte";
	import {goto} from "$app/navigation";

	let displayText = "";
	let pressedKeys = new Set<string>();
	let audioContext: AudioContext;
	let audioBuffers: AudioBuffer[] = [];
	let showNotification = false;
	let notificationMessage = "";

	// Typing speed tracking
	let typingStartTime: number | null = null;
	let charactersTyped = 0;
	let currentWPM = 0;
	let showWPM = false;
	let highSpeedStartTime: number | null = null;
	let isShaking = false;
	let confettiElements: Array<{
		id: number;
		x: number;
		y: number;
		text: string;
		color: string;
		animationType: number;
	}> = [];
	let confettiId = 0;
	let typingTimer: number | null = null;
	let lastTypingTime: number | null = null;

	// Full 60% Keyboard layout - QWERTY with proper key widths
	const keyboardLayout = [
		[
			{key: "`", label: "~", width: 1},
			{key: "1", label: "1", width: 1},
			{key: "2", label: "2", width: 1},
			{key: "3", label: "3", width: 1},
			{key: "4", label: "4", width: 1},
			{key: "5", label: "5", width: 1},
			{key: "6", label: "6", width: 1},
			{key: "7", label: "7", width: 1},
			{key: "8", label: "8", width: 1},
			{key: "9", label: "9", width: 1},
			{key: "0", label: "0", width: 1},
			{key: "-", label: "-", width: 1},
			{key: "=", label: "=", width: 1},
			{key: "backspace", label: "⌫ BackSpace", width: 2},
		],
		[
			{key: "tab", label: "Tab", width: 1.5},
			{key: "q", label: "Q", width: 1},
			{key: "w", label: "W", width: 1},
			{key: "e", label: "E", width: 1},
			{key: "r", label: "R", width: 1},
			{key: "t", label: "T", width: 1},
			{key: "y", label: "Y", width: 1},
			{key: "u", label: "U", width: 1},
			{key: "i", label: "I", width: 1},
			{key: "o", label: "O", width: 1},
			{key: "p", label: "P", width: 1},
			{key: "[", label: "[", width: 1},
			{key: "]", label: "]", width: 1},
			{key: "\\", label: "\\", width: 1.5},
		],
		[
			{key: "capslock", label: "Caps", width: 1.75},
			{key: "a", label: "A", width: 1},
			{key: "s", label: "S", width: 1},
			{key: "d", label: "D", width: 1},
			{key: "f", label: "F", width: 1},
			{key: "g", label: "G", width: 1},
			{key: "h", label: "H", width: 1},
			{key: "j", label: "J", width: 1},
			{key: "k", label: "K", width: 1},
			{key: "l", label: "L", width: 1},
			{key: ";", label: ";", width: 1},
			{key: "'", label: "'", width: 1},
			{key: "enter", label: "Enter ⏎", width: 2.25},
		],
		[
			{key: "shift", label: "Shift", width: 2.25},
			{key: "z", label: "Z", width: 1},
			{key: "x", label: "X", width: 1},
			{key: "c", label: "C", width: 1},
			{key: "v", label: "V", width: 1},
			{key: "b", label: "B", width: 1},
			{key: "n", label: "N", width: 1},
			{key: "m", label: "M", width: 1},
			{key: ",", label: ",", width: 1},
			{key: ".", label: ".", width: 1},
			{key: "/", label: "/", width: 1},
			{key: "shift", label: "Shift", width: 2.75},
		],
		[
			{key: "control", label: "Ctrl", width: 1.25},
			{key: "meta", label: "Win", width: 1.25},
			{key: "alt", label: "Alt", width: 1.25},
			{key: " ", label: "__________", width: 7.5, isSpace: true},
			{key: "alt", label: "Alt", width: 1.25},
			{key: "meta", label: "Win", width: 1.25},
			{key: "control", label: "Ctrl", width: 1.25},
		],
	];

	// Motivational quotes for high-speed typing
	const speedQuotes = [
		"Lightning fast!",
		"Speed demon!",
		"Blazing fingers!",
		"Type warrior!",
		"Keyboard master!",
		"Velocity champion!",
		"Rapid fire!",
		"Turbo mode!",
		"Flying fingers!",
		"Speed king/queen!",
		"Warp speed!",
		"Sonic typing!",
		"Hyperspeed!",
		"Velocity virtuoso!",
		"Rapid response!",
	];

	// Define handled keywords - add new keywords here
	const handledKeywords: Record<string, () => void> = {
		foglomon: () => goto("/portfolio"),
		github: () => {
			window.open("https://github.com/foglomon", "_self");
			displayText = "";
		},
		help: () => {
			showNotificationPopup(
				"Try: 'foglomon', 'github', 'sudo get-job', 'whoami','quote'."
			);
			displayText = "";
			setTimeout(() => {
				showNotificationPopup("Psst. These are not all!");
			}, 3000);
		},
		"sudo get-job": () => {
			showNotificationPopup("Searching for job opportunities... 💼");
			setTimeout(() => {
				showNotificationPopup("No jobs found. Keep trying! 🥹");
			}, 5000);
			displayText = "";
		},
		bbqr: () => {
			window.open("https://github.com/foglomon/bbqr", "_self");
			displayText = "";
		},
		// Fun easter eggs
		whoami: () => {
			showNotificationPopup("I don't know about you, but I am Ishman!");
			displayText = "";
		},
		"hello world": () => {
			showNotificationPopup("Hello, World! 🌍 Classic first program executed.");
			displayText = "";
		},
		"sudo rm -rf /": () => {
			showNotificationPopup("⚠️ Nice try! That's not happening here. 😄");
			displayText = "";
		},
		quote: () => {
			const quotes = [
				"💡 'Code never lies, comments sometimes do.' - Ron Jeffries",
				"🐛 'Debugging is twice as hard as writing code.' - Brian Kernighan",
				"✨ 'Simplicity is the ultimate sophistication.' - Leonardo da Vinci",
				"🚀 'Make it work, make it right, make it fast.' - Kent Beck",
				"🎯 'Premature optimization is the root of all evil.' - Donald Knuth",
			];
			const randomquote = quotes[Math.floor(Math.random() * quotes.length)];
			showNotificationPopup(randomquote);
			displayText = "";
		},
	};

	function showNotificationPopup(message: string) {
		// Limit message length to prevent overflow
		const maxLength = 80;
		notificationMessage =
			message.length > maxLength
				? message.substring(0, maxLength) + "..."
				: message;
		showNotification = true;

		// Hide notification after 3 seconds
		setTimeout(() => {
			showNotification = false;
		}, 3000);
	}

	function calculateWPM() {
		if (!typingStartTime || charactersTyped === 0) return 0;

		const currentTime = Date.now();
		const timeElapsed = (currentTime - typingStartTime) / 1000 / 60; // Convert to minutes
		const wordsTyped = charactersTyped / 5; // Standard: 5 characters = 1 word

		return Math.round(wordsTyped / timeElapsed);
	}

	function updateTypingSpeed() {
		currentWPM = calculateWPM();

		// Only show WPM and effects after 3 seconds of typing
		const currentTime = Date.now();
		const hasTypedFor3Seconds =
			typingStartTime && currentTime - typingStartTime >= 3000;

		showWPM = !!hasTypedFor3Seconds && currentWPM > 40;
		lastTypingTime = currentTime;

		// Check for high-speed effects (only after 3 seconds)
		if (hasTypedFor3Seconds && currentWPM >= 40) {
			triggerConfetti();
		}

		// Check for extreme speed (100+ WPM)
		if (currentWPM >= 100) {
			if (!highSpeedStartTime) {
				highSpeedStartTime = Date.now();
			} else if (Date.now() - highSpeedStartTime >= 5000) {
				// 5 seconds
				if (!isShaking) {
					isShaking = true;
				}
			}
		} else {
			highSpeedStartTime = null;
			isShaking = false;
		}

		// Clear existing timer
		if (typingTimer) {
			clearTimeout(typingTimer);
		}

		// Set timer to reset WPM and shaking after 3 seconds of no typing
		typingTimer = setTimeout(() => {
			if (lastTypingTime && Date.now() - lastTypingTime >= 3000) {
				resetTypingStats();
			}
		}, 3000);
	}

	function triggerConfetti() {
		// Don't spam confetti - limit to every 500ms
		if (confettiElements.length > 8) return;

		const quote = speedQuotes[Math.floor(Math.random() * speedQuotes.length)];
		const colors = [
			"#ff6b5a",
			"#4fc3f7",
			"#66bb6a",
			"#ffb74d",
			"#ba68c8",
			"#ff8a65",
		];
		const color = colors[Math.floor(Math.random() * colors.length)];

		// Position around keyboard area instead of screen edges
		let x: number, y: number;

		if (typeof window !== "undefined") {
			// Calculate approximate keyboard position (centered in viewport)
			const keyboardWidth = Math.min(1400 * 0.85, window.innerWidth * 0.8); // Approximate keyboard width
			const keyboardHeight = 300; // Approximate keyboard height
			const centerX = window.innerWidth / 2;
			const centerY = window.innerHeight / 2 + 100; // Keyboard is slightly below center

			// Create a larger area around the keyboard for message positioning
			const padding = 200; // Distance from keyboard edges
			const areaLeft = centerX - keyboardWidth / 2 - padding;
			const areaRight = centerX + keyboardWidth / 2 + padding;
			const areaTop = centerY - keyboardHeight / 2 - padding;
			const areaBottom = centerY + keyboardHeight / 2 + padding;

			// Random position around the keyboard perimeter
			const side = Math.floor(Math.random() * 4); // 0: left, 1: right, 2: top, 3: bottom

			switch (side) {
				case 0: // Left side of keyboard area
					x = areaLeft;
					y = areaTop + Math.random() * (areaBottom - areaTop);
					break;
				case 1: // Right side of keyboard area
					x = areaRight;
					y = areaTop + Math.random() * (areaBottom - areaTop);
					break;
				case 2: // Top of keyboard area
					x = areaLeft + Math.random() * (areaRight - areaLeft);
					y = areaTop;
					break;
				case 3: // Bottom of keyboard area
					x = areaLeft + Math.random() * (areaRight - areaLeft);
					y = areaBottom;
					break;
				default:
					x = areaLeft;
					y = centerY;
			}
		} else {
			// Fallback for SSR
			x = Math.random() > 0.5 ? 300 : 700;
			y = Math.random() * 200 + 400;
		}

		const confettiItem = {
			id: confettiId++,
			x,
			y,
			text: quote,
			color,
			animationType: Math.floor(Math.random() * 3), // 0, 1, or 2 for different animation patterns
		};

		confettiElements = [...confettiElements, confettiItem];

		// Remove confetti after animation
		setTimeout(() => {
			confettiElements = confettiElements.filter(
				(item) => item.id !== confettiItem.id
			);
		}, 3000);
	}

	function resetTypingStats() {
		typingStartTime = null;
		charactersTyped = 0;
		currentWPM = 0;
		showWPM = false;
		highSpeedStartTime = null;
		isShaking = false;
		lastTypingTime = null;
		if (typingTimer) {
			clearTimeout(typingTimer);
			typingTimer = null;
		}
	}

	function handleEnterAction() {
		const inputText = displayText.toLowerCase().trim();

		if (handledKeywords[inputText]) {
			handledKeywords[inputText]();
		} else if (inputText.length > 0) {
			// Show notification for unhandled keywords
			showNotificationPopup(`"${displayText}" is not a recognized command.`);
			displayText = "";
		} else {
			displayText = "";
		}

		// Reset typing stats when command is executed
		resetTypingStats();
	}

	onMount(async () => {
		// Initialize Web Audio API
		audioContext = new (window.AudioContext ||
			(window as any).webkitAudioContext)();

		// Load sound files
		try {
			const soundFiles = [
				"/assets/sounds/mech1.m4a",
				"/assets/sounds/mech2.m4a",
				"/assets/sounds/mech3.m4a",
			];
			audioBuffers = await Promise.all(soundFiles.map(loadAudio));
		} catch (error) {
			console.warn("Could not load audio files:", error);
		}
	});

	onDestroy(() => {
		// Clean up any active timers
		if (typingTimer) {
			clearTimeout(typingTimer);
		}
	});

	async function loadAudio(url: string): Promise<AudioBuffer> {
		const response = await fetch(url);
		const arrayBuffer = await response.arrayBuffer();
		return await audioContext.decodeAudioData(arrayBuffer);
	}

	function playRandomSound() {
		if (audioBuffers.length === 0) return;

		const randomBuffer =
			audioBuffers[Math.floor(Math.random() * audioBuffers.length)];
		const source = audioContext.createBufferSource();
		source.buffer = randomBuffer;
		source.connect(audioContext.destination);
		source.start();
	}

	function handleKeyDown(event: KeyboardEvent) {
		let key = event.key.toLowerCase();

		// Map some keys to match our layout
		if (key === " ") {
			key = " ";
			event.preventDefault();
		} else if (key === "capslock") {
			key = "capslock";
		} else if (key === "tab") {
			key = "tab";
			event.preventDefault();
		} else if (key === "control") {
			key = "control";
		} else if (key === "meta") {
			key = "meta";
		} else if (key === "alt") {
			key = "alt";
		} else if (key === "shift") {
			key = "shift";
		}

		// Only handle if not already pressed (prevents key repeat)
		if (pressedKeys.has(key)) return;

		// Add to pressed keys set
		pressedKeys.add(key);
		pressedKeys = pressedKeys; // Trigger reactivity

		// Play sound
		playRandomSound();

		// Handle display text
		if (key === "enter") {
			handleEnterAction();
		} else if (key === "backspace") {
			displayText = displayText.slice(0, -1);
			if (displayText.length === 0) {
				resetTypingStats();
			}
		} else if (key === " ") {
			displayText += " ";
			charactersTyped++;
			if (!typingStartTime) typingStartTime = Date.now();
			updateTypingSpeed();
		} else if (key.length === 1 && /^[a-z0-9`\-=\[\];',./\\]$/i.test(key)) {
			displayText += key;
			charactersTyped++;
			if (!typingStartTime) typingStartTime = Date.now();
			updateTypingSpeed();
		}
	}

	function handleKeyUp(event: KeyboardEvent) {
		let key = event.key.toLowerCase();

		// Map some keys to match our layout
		if (key === " ") {
			key = " ";
		} else if (key === "capslock") {
			key = "capslock";
		} else if (key === "tab") {
			key = "tab";
		} else if (key === "control") {
			key = "control";
		} else if (key === "meta") {
			key = "meta";
		} else if (key === "alt") {
			key = "alt";
		} else if (key === "shift") {
			key = "shift";
		}

		pressedKeys.delete(key);
		pressedKeys = pressedKeys; // Trigger reactivity
	}

	function handleKeyClick(key: string) {
		// Don't handle if already pressed
		if (pressedKeys.has(key)) return;

		// Add visual feedback
		pressedKeys.add(key);
		pressedKeys = pressedKeys;

		// Play sound
		playRandomSound();

		// Handle display text
		if (key === "enter") {
			handleEnterAction();
		} else if (key === "backspace") {
			displayText = displayText.slice(0, -1);
			if (displayText.length === 0) {
				resetTypingStats();
			}
		} else if (key === " ") {
			displayText += " ";
			charactersTyped++;
			if (!typingStartTime) typingStartTime = Date.now();
			updateTypingSpeed();
		} else if (key.length === 1 && /^[a-z0-9`\-=\[\];',./\\]$/i.test(key)) {
			displayText += key;
			charactersTyped++;
			if (!typingStartTime) typingStartTime = Date.now();
			updateTypingSpeed();
		}

		// Remove visual feedback after a short delay
		setTimeout(() => {
			pressedKeys.delete(key);
			pressedKeys = pressedKeys;
		}, 150);
	}
</script>

<svelte:window on:keydown={handleKeyDown} on:keyup={handleKeyUp} />

<div class="scene" class:shaking={isShaking}>
	<!-- Confetti Elements -->
	{#each confettiElements as confetti (confetti.id)}
		<div
			class="confetti confetti-animation-{confetti.animationType}"
			style="
				left: {confetti.x}px; 
				top: {confetti.y}px; 
				color: {confetti.color};
				--start-x: {confetti.x}px;
				--start-y: {confetti.y}px;
			"
		>
			{confetti.text}
		</div>
	{/each}

	<div class="table">
		<div class="display-container">
			<!-- WPM Display -->
			{#if showWPM}
				<div class="wpm-display">
					{currentWPM} WPM
				</div>
			{/if}

			<div class="eink-display">
				<div class="display-content">
					{#if displayText.length === 0}
						<div class="prompt-container">
							<div class="prompt">Type "foglomon" and press Enter</div>
							<div class="prompt">Use "Help" for more info.</div>
						</div>
					{:else}
						<div class="typed-text">{displayText}</div>
					{/if}
				</div>
			</div>

			<!-- Notification Popup -->
			{#if showNotification}
				<div class="notification-popup" class:show={showNotification}>
					<div class="notification-content">
						<div class="notification-icon">⚠️</div>
						<div class="notification-message">{notificationMessage}</div>
					</div>
				</div>
			{/if}
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
								on:click={() => handleKeyClick(keyData.key)}
								on:keydown={(e) =>
									e.key === "Enter" && handleKeyClick(keyData.key)}
								role="button"
								tabindex="0"
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
	.scene {
		margin: 0;
		padding: 0;
		overflow: hidden;
		font-family: "Courier New", monospace;
		width: 100vw;
		height: 100vh;
		background: url("/assets/images/wood.jpg");
		background-size: cover;
		background-position: center;
		background-repeat: no-repeat;
		background-attachment: fixed;
		display: flex;
		justify-content: center;
		align-items: center;
		perspective: 1000px;
		position: relative;
	}

	.scene::before {
		content: "";
		position: absolute;
		top: 0;
		left: 50%;
		transform: translateX(-50%);
		width: 80%;
		height: 60%;
		background: radial-gradient(
			ellipse at center,
			rgba(255, 255, 220, 0.3) 0%,
			transparent 70%
		);
		pointer-events: none;
		z-index: 1;
	}

	.scene.shaking {
		animation: screenShake 0.5s infinite;
	}

	@keyframes screenShake {
		0%,
		100% {
			transform: translate(0);
		}
		10% {
			transform: translate(-2px, -1px);
		}
		20% {
			transform: translate(2px, 1px);
		}
		30% {
			transform: translate(-1px, 2px);
		}
		40% {
			transform: translate(1px, -2px);
		}
		50% {
			transform: translate(-2px, 1px);
		}
		60% {
			transform: translate(2px, -1px);
		}
		70% {
			transform: translate(-1px, -2px);
		}
		80% {
			transform: translate(1px, 2px);
		}
		90% {
			transform: translate(-2px, -1px);
		}
	}

	.wpm-display {
		position: absolute;
		top: -30px;
		right: 0;
		color: white;
		background: rgba(0, 0, 0, 0.7);
		padding: 4px 8px;
		border-radius: 4px;
		font-family: "Courier New", monospace;
		font-weight: bold;
		font-size: 14px;
		text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
		z-index: 1001;
		animation: wpmGlow 2s ease-in-out infinite;
	}

	@keyframes wpmGlow {
		0%,
		100% {
			text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
		}
		50% {
			text-shadow: 0 0 20px rgba(255, 255, 255, 0.8);
		}
	}

	.confetti {
		position: fixed;
		font-weight: bold;
		font-size: 16px;
		pointer-events: none;
		z-index: 999;
		text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
		white-space: nowrap;
	}

	.confetti-animation-0 {
		animation: confettiFloat0 3s ease-out forwards;
	}

	.confetti-animation-1 {
		animation: confettiFloat1 3s ease-out forwards;
	}

	.confetti-animation-2 {
		animation: confettiFloat2 3s ease-out forwards;
	}

	@keyframes confettiFloat0 {
		0% {
			opacity: 1;
			transform: translate(0, 0) scale(0.5) rotate(0deg);
		}
		25% {
			opacity: 1;
			transform: translate(-30px, -20px) scale(1) rotate(90deg);
		}
		50% {
			opacity: 1;
			transform: translate(10px, -40px) scale(1.2) rotate(180deg);
		}
		75% {
			opacity: 0.8;
			transform: translate(40px, -25px) scale(1) rotate(270deg);
		}
		100% {
			opacity: 0;
			transform: translate(60px, 10px) scale(0.6) rotate(360deg);
		}
	}

	@keyframes confettiFloat1 {
		0% {
			opacity: 1;
			transform: translate(0, 0) scale(0.5);
		}
		33% {
			opacity: 1;
			transform: translate(50px, -30px) scale(1.1);
		}
		66% {
			opacity: 0.9;
			transform: translate(-20px, -60px) scale(1.2);
		}
		100% {
			opacity: 0;
			transform: translate(80px, -20px) scale(0.7);
		}
	}

	@keyframes confettiFloat2 {
		0% {
			opacity: 1;
			transform: translate(0, 0) scale(0.5) rotate(0deg);
		}
		20% {
			opacity: 1;
			transform: translate(-40px, 10px) scale(0.9) rotate(-45deg);
		}
		40% {
			opacity: 1;
			transform: translate(-60px, -30px) scale(1.2) rotate(-90deg);
		}
		60% {
			opacity: 1;
			transform: translate(-30px, -50px) scale(1.1) rotate(-135deg);
		}
		80% {
			opacity: 0.7;
			transform: translate(20px, -40px) scale(0.9) rotate(-180deg);
		}
		100% {
			opacity: 0;
			transform: translate(70px, -10px) scale(0.6) rotate(-225deg);
		}
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
		content: "";
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
		overflow-y: auto;
		overflow-x: hidden;
	}

	.prompt-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 8px;
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
		word-wrap: break-word;
		word-break: break-word;
		hyphens: auto;
		position: relative;
		max-width: 100%;
		line-height: 1.3;
	}

	.typed-text::after {
		content: "|";
		animation: blink 1s infinite;
		margin-left: 2px;
	}

	@keyframes blink {
		0%,
		50% {
			opacity: 1;
		}
		51%,
		100% {
			opacity: 0;
		}
	}

	@keyframes pulse {
		0%,
		100% {
			opacity: 1;
		}
		50% {
			opacity: 0.5;
		}
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
		content: "";
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
		background: linear-gradient(145deg, #daa520, #b8860b);
		border: 2px solid #8b7355;
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
		content: "";
		position: absolute;
		top: 1px;
		left: 1px;
		right: 1px;
		bottom: 1px;
		background: linear-gradient(145deg, rgba(255, 255, 255, 0.1), transparent);
		border-radius: 4px;
		pointer-events: none;
	}

	.key.pressed .keycap {
		transform: translateY(2px);
		box-shadow:
			0 2px 4px rgba(0, 0, 0, 0.3),
			inset 0 1px 2px rgba(255, 255, 255, 0.3),
			inset 0 -1px 2px rgba(0, 0, 0, 0.2);
		background: linear-gradient(145deg, #b8860b, #daa520);
	}

	.keycap:hover {
		transform: translateY(1px);
	}

	/* Add custom scrollbar styling for webkit browsers */
	.display-content::-webkit-scrollbar {
		width: 6px;
	}

	.display-content::-webkit-scrollbar-track {
		background: rgba(0, 0, 0, 0.1);
		border-radius: 3px;
	}

	.display-content::-webkit-scrollbar-thumb {
		background: rgba(0, 0, 0, 0.3);
		border-radius: 3px;
	}

	.display-content::-webkit-scrollbar-thumb:hover {
		background: rgba(0, 0, 0, 0.5);
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

		.notification-popup {
			width: 90%;
			min-width: 250px;
			max-width: 450px;
		}

		.notification-content {
			padding: 10px 16px;
		}

		.notification-message {
			font-size: 12px;
		}

		.wpm-display {
			top: -25px;
			right: 0;
			font-size: 12px;
		}

		.confetti {
			font-size: 12px;
		}
	}

	/* Notification Popup Styles */
	.notification-popup {
		position: absolute;
		top: -80px;
		left: 50%;
		transform: translateX(-50%) translateY(-20px);
		background: rgba(255, 69, 58, 0.95);
		border: 2px solid #ff453a;
		border-radius: 12px;
		box-shadow:
			0 10px 30px rgba(255, 69, 58, 0.3),
			0 4px 15px rgba(0, 0, 0, 0.2);
		z-index: 100;
		opacity: 0;
		animation: slideInDown 0.3s ease-out forwards;
		backdrop-filter: blur(10px);
		min-width: 350px;
		max-width: 1000px;
		width: auto;
	}

	.notification-popup.show {
		animation: slideInDown 0.3s ease-out forwards;
	}

	.notification-content {
		display: flex;
		align-items: center;
		gap: 12px;
		padding: 12px 24px;
		color: white;
	}

	.notification-icon {
		font-size: 20px;
		flex-shrink: 0;
	}

	.notification-message {
		font-size: 14px;
		font-weight: 500;
		line-height: 1.4;
		text-align: left;
		word-wrap: break-word;
		overflow-wrap: break-word;
		flex: 1;
		min-width: 0;
	}

	@keyframes slideInDown {
		0% {
			opacity: 0;
			transform: translateX(-50%) translateY(-40px);
		}
		100% {
			opacity: 1;
			transform: translateX(-50%) translateY(0);
		}
	}

	/* Add a subtle pulse effect for extra attention */
	.notification-popup::before {
		content: "";
		position: absolute;
		top: -2px;
		left: -2px;
		right: -2px;
		bottom: -2px;
		background: linear-gradient(45deg, #ff453a, #ff6b5a, #ff453a);
		border-radius: 14px;
		z-index: -1;
		animation: pulse-border 2s ease-in-out infinite;
	}

	@keyframes pulse-border {
		0%,
		100% {
			opacity: 0.8;
		}
		50% {
			opacity: 1;
		}
	}
</style>
