<script lang="ts">
	import {onMount} from "svelte";
	import {browser} from "$app/environment";

	let mounted = false;
	let scrollY = 0;
	let innerHeight = 0;

	onMount(() => {
		mounted = true;

		// Animate path drawing and content on scroll
		const handleScroll = () => {
			scrollY = window.scrollY;

			// Animate path based on scroll
			const pathElement = document.querySelector(".journey-path svg path");
			const markerElement = document.querySelector(".journey-marker");
			if (pathElement && markerElement) {
				const scrollPercent = Math.min(
					scrollY / (document.body.scrollHeight - window.innerHeight),
					1
				);
				const pathLength = (pathElement as SVGPathElement).getTotalLength();
				(pathElement as SVGPathElement).style.strokeDasharray = `${pathLength}`;
				(pathElement as SVGPathElement).style.strokeDashoffset =
					`${pathLength * (1 - scrollPercent)}`;

				// Move the marker along the path
				const markerPosition = scrollPercent * (window.innerHeight - 20);
				(markerElement as HTMLElement).style.top = `${markerPosition}px`;
			} // Fade hero text on scroll
			const heroContent = document.querySelector(".hero-content");
			if (heroContent) {
				const fadeStart = 0;
				const fadeEnd = window.innerHeight * 0.8;
				const fadeProgress = Math.min(
					Math.max((scrollY - fadeStart) / (fadeEnd - fadeStart), 0),
					1
				);
				const opacity = 1 - fadeProgress;
				const translateY = fadeProgress * 50;

				(heroContent as HTMLElement).style.opacity = opacity.toString();
				(heroContent as HTMLElement).style.transform =
					`translateY(${translateY}px)`;
			}

			// Animate sections based on scroll position with smoother transitions
			const sections = document.querySelectorAll(".scroll-section");
			sections.forEach((section, index) => {
				const rect = section.getBoundingClientRect();
				const isVisible =
					rect.top < window.innerHeight * 0.9 &&
					rect.bottom > window.innerHeight * 0.1;

				if (isVisible) {
					section.classList.add("visible");
				}
			});
		};

		if (browser) {
			window.addEventListener("scroll", handleScroll);
			handleScroll(); // Initial call

			return () => {
				window.removeEventListener("scroll", handleScroll);
			};
		}
	});
</script>

<svelte:window bind:scrollY bind:innerHeight />

<div class="portfolio-container" class:mounted>
	<!-- The Journey Path SVG -->
	<div class="journey-path">
		<svg
			width="4"
			height="100%"
			viewBox="0 0 4 2000"
			preserveAspectRatio="none"
		>
			<path
				d="M2,0 L2,2000"
				stroke="#667eea"
				stroke-width="2"
				fill="none"
				stroke-linecap="round"
			/>
		</svg>
		<div class="journey-marker"></div>
	</div>

	<!-- The Hook (Initial View) -->
	<section class="hero scroll-section">
		<div class="hero-content">
			<h1 class="hero-title">Hi, I'm Ishman.</h1>
			<p class="hero-subtitle">I turn ideas into reality through code.</p>
			<div class="scroll-indicator">
				<div class="scroll-arrow"></div>
			</div>
		</div>
	</section>

	<!-- Scroll 1: About Me -->
	<section class="about scroll-section">
		<div class="container">
			<div class="section-content">
				<h2 class="section-title">About Me</h2>
				<div class="about-content">
					<div class="about-text">
						<p>
							I'm Ishman, a student, developer and keyboard enthusiast who
							loves creating innovative solutions and tinkering with technology.
							Currently, I'm working on <strong>ezcalc</strong> (A simplistic
							scientific calculator) and diving deep into
							<strong>PCB Design</strong> - always learning something new!
						</p>
						<p>
							I work with a wide range of technologies from low-level
							programming (C/C++) to modern web development
							(JavaScript/TypeScript, React, Svelte), mobile development
							(Flutter/Dart), and even hardware projects with Raspberry Pi.
						</p>
						<p>
							When I'm not coding, you'll find me gaming, designing mechanical
							keyboards, experimenting in the kitchen (I make killer food! 🍳),
							or exploring 3D modeling (though I could definitely use some help
							there 😭). I believe in clean, maintainable code and creating
							intuitive user experiences.
						</p>
					</div>
				</div>
			</div>
		</div>
	</section>

	<!-- Scroll 2: Building the Foundation -->
	<section class="skills scroll-section">
		<div class="container">
			<div class="section-content">
				<h2 class="section-title">Building the Foundation</h2>
				<p class="section-subtitle">Technologies I've mastered on my journey</p>
				<div class="skills-constellation">
					<div class="skill-group frontend">
						<h3>Frontend</h3>
						<div class="skill-items">
							<div class="skill-item" data-delay="0">JavaScript</div>
							<div class="skill-item" data-delay="100">TypeScript</div>
							<div class="skill-item" data-delay="200">React</div>
							<div class="skill-item" data-delay="300">Svelte</div>
							<div class="skill-item" data-delay="400">HTML5</div>
							<div class="skill-item" data-delay="500">CSS3</div>
						</div>
					</div>
					<div class="skill-group backend">
						<h3>Backend</h3>
						<div class="skill-items">
							<div class="skill-item" data-delay="600">Node.js</div>
							<div class="skill-item" data-delay="700">Python</div>
							<div class="skill-item" data-delay="800">C/C++</div>
							<div class="skill-item" data-delay="900">MySQL</div>
							<div class="skill-item" data-delay="1000">Docker</div>
						</div>
					</div>
					<div class="skill-group cloud">
						<h3>Cloud & Tools</h3>
						<div class="skill-items">
							<div class="skill-item" data-delay="1100">AWS</div>
							<div class="skill-item" data-delay="1200">Firebase</div>
							<div class="skill-item" data-delay="1300">Git</div>
							<div class="skill-item" data-delay="1400">GitHub</div>
							<div class="skill-item" data-delay="1500">Cloudflare</div>
							<div class="skill-item" data-delay="1600">Raspberry Pi</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</section>

	<!-- Scroll 3: Putting Skills into Practice -->
	<section class="projects scroll-section">
		<div class="container">
			<div class="section-content">
				<h2 class="section-title">Putting Skills into Practice</h2>
				<p class="section-subtitle">Featured work that defines my journey</p>

				<div class="project-showcase">
					<a
						href="https://github.com/foglomon/bbqr"
						class="project-link"
						target="_blank"
						rel="noopener"
					>
						<div class="project-item">
							<div class="project-visual">
								<div class="project-icon">🥩</div>
								<div class="project-bg"></div>
							</div>
							<div class="project-details">
								<h3>BBQR (Barbequer)</h3>
								<p>
									A BBQ-themed terminal QR code generator that "grills your data
									to perfection"! Features cross-platform WiFi QR codes, file
									upload/sharing, and multi-threaded operations.
								</p>
								<div class="project-tech">
									<span>Python</span>
									<span>QR Codes</span>
									<span>CLI</span>
								</div>
							</div>
						</div>
					</a>

					<a
						href="https://github.com/foglomon/TerminalWriter"
						class="project-link"
						target="_blank"
						rel="noopener"
					>
						<div class="project-item">
							<div class="project-visual">
								<div class="project-icon">⌨️</div>
								<div class="project-bg"></div>
							</div>
							<div class="project-details">
								<h3>TerminalWriter</h3>
								<p>
									A terminal-based text editor with modern features like syntax
									highlighting, multiple tabs, and customizable themes. Built
									for developers who live in the terminal.
								</p>
								<div class="project-tech">
									<span>Python</span>
									<span>Terminal UI</span>
									<span>Text Editor</span>
								</div>
							</div>
						</div>
					</a>

					<a
						href="https://github.com/foglomon/life-points"
						class="project-link"
						target="_blank"
						rel="noopener"
					>
						<div class="project-item">
							<div class="project-visual">
								<img
									src="/assets/images/lifepoints.png"
									alt="Life Points"
									class="project-full-image"
								/>
							</div>
							<div class="project-details">
								<h3>Life Points</h3>
								<p>
									A Flutter mobile app that gamifies productivity by rewarding
									points for task completion. Features offline-first design,
									deadline tracking, and penalty systems.
								</p>
								<div class="project-tech">
									<span>Flutter</span>
									<span>Dart</span>
									<span>Mobile</span>
								</div>
							</div>
						</div>
					</a>
				</div>
			</div>
		</div>
	</section>

	<!-- Scroll 4: The Call to Action -->
	<section class="contact scroll-section">
		<div class="container">
			<div class="section-content">
				<h2 class="section-title final-title">
					Let's create something together
				</h2>
				<p class="contact-description">
					Whether you have a project in mind, want to collaborate, or just want
					to chat about tech and keyboards, I'd love to hear from you.
				</p>
				<div class="contact-grid">
					<a href="mailto:ishmansingh@duck.com" class="contact-card">
						<div class="contact-icon">📧</div>
						<h3>Email</h3>
						<p>ishmansingh@duck.com</p>
					</a>
					<a
						href="https://github.com/foglomon"
						class="contact-card"
						target="_blank"
						rel="noopener"
					>
						<div class="contact-icon">
							<img
								src="/assets/images/github.png"
								alt="GitHub"
								class="github-icon"
							/>
						</div>
						<h3>GitHub</h3>
						<p>@foglomon</p>
					</a>
				</div>
			</div>
		</div>
	</section>

	<footer class="footer">
		<div class="container">
			<p>&copy; 2025 Ishman Singh. Crafted with &hearts; and code.</p>
			<a href="/" class="back-link">← Back to Keyboard</a>
		</div>
	</footer>
</div>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		background: #0a0a0a;
		color: #ffffff;
		overflow-x: hidden;
		scrollbar-width: none;
		-ms-overflow-style: none;
	}

	:global(body::-webkit-scrollbar) {
		display: none;
	}

	.portfolio-container {
		margin: 0;
		padding: 0;
		font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
			Ubuntu, Cantarell, sans-serif;
		line-height: 1.6;
		background: linear-gradient(
			180deg,
			#0a0a0a 0%,
			#1a1a2e 15%,
			#16213e 25%,
			#1a1a2e 35%,
			#2d1b69 45%,
			#16213e 55%,
			#2d1b69 65%,
			#1a1a2e 75%,
			#16213e 85%,
			#0a0a0a 95%,
			#000000 100%
		);
		color: #ffffff;
		opacity: 0;
		transition: opacity 1s ease;
		position: relative;
		min-height: 100vh;
	}

	.portfolio-container.mounted {
		opacity: 1;
	}

	/* Journey Path */
	.journey-path {
		position: fixed;
		left: 2rem;
		top: 0;
		height: 100vh;
		z-index: 10;
		pointer-events: none;
	}

	.journey-path svg {
		height: 100%;
	}

	.journey-path path {
		stroke-dasharray: 2000;
		stroke-dashoffset: 2000;
		transition: stroke-dashoffset 0.3s ease-out;
	}

	.journey-marker {
		position: absolute;
		left: 50%;
		top: 0;
		width: 8px;
		height: 8px;
		background: #667eea;
		border-radius: 50%;
		transform: translateX(-50%);
		transition: top 0.3s ease-out;
		box-shadow: 0 0 10px rgba(102, 126, 234, 0.5);
	}

	.container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 4rem;
	}

	/* Scroll Sections */
	.scroll-section {
		min-height: 100vh;
		display: flex;
		align-items: center;
		opacity: 0;
		transform: translateY(50px);
		transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
		position: relative;
		padding: 5vh 0;
	}

	:global(.scroll-section.visible) {
		opacity: 1;
		transform: translateY(0);
	}

	.section-content {
		width: 100%;
		max-width: 800px;
		margin: 0 auto;
		text-align: center;
	}

	.section-title {
		font-size: 3rem;
		font-weight: 700;
		margin-bottom: 2rem;
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.section-subtitle {
		font-size: 1.2rem;
		color: #888;
		margin-bottom: 3rem;
	}

	/* Hero Section */
	.hero {
		background: transparent;
		position: relative;
		justify-content: center;
		text-align: center;
		z-index: 2;
	}

	.hero-content {
		transition:
			opacity 0.1s ease-out,
			transform 0.1s ease-out;
	}

	.hero-title {
		font-size: 4.5rem;
		font-weight: 800;
		margin-bottom: 1.5rem;
		color: #ffffff;
		text-shadow: 0 0 30px rgba(102, 126, 234, 0.3);
		animation: glow 2s ease-in-out infinite alternate;
	}

	.hero-subtitle {
		font-size: 1.8rem;
		color: #ccc;
		margin-bottom: 4rem;
		font-weight: 300;
	}

	@keyframes glow {
		from {
			text-shadow: 0 0 30px rgba(102, 126, 234, 0.3);
		}
		to {
			text-shadow:
				0 0 50px rgba(102, 126, 234, 0.5),
				0 0 70px rgba(118, 75, 162, 0.3);
		}
	}

	.scroll-indicator {
		position: absolute;
		bottom: 2rem;
		left: 50%;
		transform: translateX(-50%);
		animation: bounce 2s infinite;
	}

	.scroll-arrow {
		width: 2px;
		height: 30px;
		background: linear-gradient(to bottom, transparent, #667eea);
		position: relative;
	}

	.scroll-arrow::after {
		content: "";
		position: absolute;
		bottom: 0;
		left: -4px;
		width: 0;
		height: 0;
		border-left: 5px solid transparent;
		border-right: 5px solid transparent;
		border-top: 8px solid #667eea;
	}

	@keyframes bounce {
		0%,
		20%,
		50%,
		80%,
		100% {
			transform: translateX(-50%) translateY(0);
		}
		40% {
			transform: translateX(-50%) translateY(-10px);
		}
		60% {
			transform: translateX(-50%) translateY(-5px);
		}
	}

	/* About Section */
	.about {
		background: transparent;
		z-index: 1;
	}

	.about-content {
		max-width: 800px;
		margin: 0 auto;
		text-align: left;
	}

	.about-text p {
		font-size: 1.2rem;
		margin-bottom: 1.5rem;
		color: #ddd;
		line-height: 1.8;
	}

	.about-text strong {
		color: #667eea;
		font-weight: 600;
	}

	/* Skills Section */
	.skills {
		background: transparent;
		z-index: 1;
	}

	.skills-constellation {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 4rem;
		text-align: left;
	}

	.skill-group h3 {
		font-size: 1.5rem;
		margin-bottom: 2rem;
		color: #667eea;
		text-align: center;
	}

	.skill-items {
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.skill-item {
		background: rgba(102, 126, 234, 0.1);
		border: 1px solid rgba(102, 126, 234, 0.3);
		padding: 1rem 1.5rem;
		border-radius: 10px;
		font-weight: 500;
		transform: translateX(-50px);
		opacity: 0;
		transition: all 0.6s ease;
		position: relative;
		overflow: hidden;
	}

	.skill-item::before {
		content: "";
		position: absolute;
		top: 0;
		left: -100%;
		width: 100%;
		height: 100%;
		background: linear-gradient(
			90deg,
			transparent,
			rgba(255, 255, 255, 0.1),
			transparent
		);
		transition: left 0.5s ease;
	}

	.skill-item:hover::before {
		left: 100%;
	}

	:global(.skills.visible .skill-item) {
		transform: translateX(0);
		opacity: 1;
		animation: slideInRight 0.6s ease forwards;
	}

	@keyframes slideInRight {
		from {
			transform: translateX(-50px);
			opacity: 0;
		}
		to {
			transform: translateX(0);
			opacity: 1;
		}
	}

	/* Projects Section */
	.projects {
		background: transparent;
		z-index: 1;
	}

	.project-showcase {
		display: flex;
		flex-direction: column;
		gap: 6rem;
	}

	.project-item {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 4rem;
		align-items: center;
	}

	.project-item:nth-child(even),
	.project-link:nth-child(even) .project-item {
		direction: rtl;
	}

	.project-item:nth-child(even) > *,
	.project-link:nth-child(even) .project-item > * {
		direction: ltr;
	}

	.project-visual {
		position: relative;
		height: 300px;
		border-radius: 20px;
		overflow: hidden;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.project-bg {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: linear-gradient(135deg, #667eea, #764ba2);
		opacity: 1;
	}

	.project-item:nth-child(1) .project-bg,
	.project-link:nth-child(1) .project-item .project-bg {
		background: linear-gradient(135deg, #ff6b6b, #ff5722);
	}

	.project-item:nth-child(2) .project-bg,
	.project-link:nth-child(2) .project-item .project-bg {
		background: linear-gradient(135deg, #4ecdc4, #26a69a);
	}

	.project-item:nth-child(3) .project-bg,
	.project-link:nth-child(3) .project-item .project-bg {
		background: transparent;
	}

	.project-icon {
		font-size: 5rem;
		position: relative;
		z-index: 2;
		filter: drop-shadow(0 4px 20px rgba(0, 0, 0, 0.5));
	}

	.project-details {
		text-align: left;
	}

	.project-details h3 {
		font-size: 2rem;
		margin-bottom: 1rem;
		color: #ffffff;
	}

	.project-details p {
		font-size: 1.1rem;
		color: #ccc;
		margin-bottom: 2rem;
		line-height: 1.6;
	}

	.project-tech {
		display: flex;
		flex-wrap: wrap;
		gap: 0.5rem;
	}

	.project-tech span {
		background: rgba(102, 126, 234, 0.2);
		border: 1px solid rgba(102, 126, 234, 0.4);
		color: #667eea;
		padding: 0.5rem 1rem;
		border-radius: 20px;
		font-size: 0.9rem;
		font-weight: 500;
	}

	/* Project Link - Invisible clickable area */
	.project-link {
		text-decoration: none;
		color: inherit;
		display: block;
		transition: all 0.3s ease;
		cursor: pointer;
	}

	.project-link:hover {
		transform: translateY(-5px);
	}

	.project-link:hover .project-visual {
		transform: scale(1.05);
	}

	/* Contact Section */
	.contact {
		background: transparent;
		position: relative;
		z-index: 1;
	}

	.final-title {
		font-size: 3.5rem;
		margin-bottom: 2rem;
		position: relative;
	}

	.final-title::after {
		content: "";
		position: absolute;
		bottom: -10px;
		left: 50%;
		transform: translateX(-50%);
		width: 200px;
		height: 3px;
		background: linear-gradient(90deg, #667eea, #764ba2);
		border-radius: 2px;
	}

	.contact-description {
		font-size: 1.3rem;
		color: #ccc;
		margin-bottom: 4rem;
		max-width: 600px;
		margin-left: auto;
		margin-right: auto;
	}

	.contact-grid {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: 2rem;
		max-width: 600px;
		margin: 0 auto;
	}

	.contact-card {
		background: rgba(102, 126, 234, 0.1);
		border: 1px solid rgba(102, 126, 234, 0.3);
		padding: 2rem;
		border-radius: 15px;
		text-decoration: none;
		color: inherit;
		transition: all 0.3s ease;
		text-align: center;
		position: relative;
		overflow: hidden;
	}

	.contact-card::before {
		content: "";
		position: absolute;
		top: 0;
		left: -100%;
		width: 100%;
		height: 100%;
		background: linear-gradient(
			90deg,
			transparent,
			rgba(102, 126, 234, 0.1),
			transparent
		);
		transition: left 0.5s ease;
	}

	.contact-card:hover::before {
		left: 100%;
	}

	.contact-card:hover {
		transform: translateY(-5px);
		border-color: rgba(102, 126, 234, 0.6);
		box-shadow: 0 10px 30px rgba(102, 126, 234, 0.2);
	}

	.contact-icon {
		font-size: 2.5rem;
		margin-bottom: 1rem;
	}

	.github-icon {
		width: 3rem;
		height: 3rem;
		object-fit: contain;
		filter: brightness(0) invert(1);
	}

	.contact-card h3 {
		font-size: 1.3rem;
		margin-bottom: 0.5rem;
		color: #667eea;
	}

	.contact-card p {
		color: #ccc;
		margin: 0;
	}

	/* Footer */
	.footer {
		background: transparent;
		padding: 3rem 0;
		border-top: 1px solid #333;
		text-align: center;
	}

	.footer .container {
		display: flex;
		justify-content: space-between;
		align-items: center;
		flex-wrap: wrap;
		gap: 1rem;
	}

	.back-link {
		color: #667eea;
		text-decoration: none;
		transition: color 0.3s ease;
		font-weight: 500;
	}

	.back-link:hover {
		color: #764ba2;
	}

	/* Responsive Design */
	@media (max-width: 1024px) {
		.container {
			padding: 0 2rem;
		}

		.journey-path {
			left: 1rem;
		}

		.skills-constellation {
			grid-template-columns: 1fr;
			gap: 3rem;
		}

		.project-item {
			grid-template-columns: 1fr;
			text-align: center;
		}

		.project-item:nth-child(even),
		.project-link:nth-child(even) .project-item {
			direction: ltr;
		}

		.about-content {
			text-align: center;
		}
	}

	@media (max-width: 768px) {
		.hero-title {
			font-size: 3rem;
		}

		.hero-subtitle {
			font-size: 1.3rem;
		}

		.section-title {
			font-size: 2.5rem;
		}

		.final-title {
			font-size: 2.8rem;
		}

		.contact-grid {
			grid-template-columns: 1fr;
		}

		.footer .container {
			flex-direction: column;
		}

		.journey-path {
			display: none;
		}
	}

	* {
		box-sizing: border-box;
	}
</style>
