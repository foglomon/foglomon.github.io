<script lang="ts">
	import {onMount} from "svelte";

	let mounted = false;

	onMount(() => {
		mounted = true;
	});
</script>

<div class="portfolio-container" class:mounted>
	<header class="hero">
		<div class="hero-content">
			<h1 class="name">Hi! I'm Ishman</h1>
			<p class="tagline">A Student, Developer and Keyboard Enthusiast</p>
			<div class="hero-actions">
				<a href="#about" class="btn primary">About Me</a>
				<a href="#projects" class="btn secondary">View Projects</a>
			</div>
		</div>
		<div class="image-disclaimer">Captured from Marvel's Spider-Man 2</div>
	</header>

	<section id="about" class="about">
		<div class="container">
			<h2>About Me</h2>
			<div class="about-content">
				<div class="about-text">
					<p>
						Well, I'm not Spider-Man (Even though I'd love to be!). That was a
						capture from on of my many gaming sessions where I play various
						different new releases or old golds...
					</p>
					<h3>Wait, I'm going too off-track. Let's start over.</h3>
					<hr />
					<p>
						I'm Ishman, a passionate developer and keyboard enthusiast who loves
						creating innovative solutions and tinkering with technology.
						Currently, I'm working on <strong>ezcalc</strong> and diving deep
						into <strong>PCB Design</strong> - always learning something new!
					</p>
					<p>
						I work with a wide range of technologies from low-level programming
						(C/C++) to modern web development (JavaScript/TypeScript, React,
						Svelte), mobile development (Flutter/Dart), and even hardware
						projects with Raspberry Pi.
					</p>
					<p>
						When I'm not coding, you'll find me gaming, designing mechanical
						keyboards, experimenting in the kitchen (I make killer food! 🍳), or
						exploring 3D modeling (though I could definitely use some help there
						😭). I believe in clean, maintainable code and creating intuitive
						user experiences.
					</p>
				</div>
				<div class="skills">
					<h3>Skills & Technologies</h3>
					<div class="skill-grid">
						<div class="skill-item">JavaScript</div>
						<div class="skill-item">TypeScript</div>
						<div class="skill-item">React/Svelte</div>
						<div class="skill-item">Node.js</div>
						<div class="skill-item">Python</div>
						<div class="skill-item">C / C++ / C#</div>
						<div class="skill-item">Dart</div>
						<div class="skill-item">HTML5</div>
						<div class="skill-item">CSS3</div>
						<div class="skill-item">Bash Script</div>
						<div class="skill-item">AWS</div>
						<div class="skill-item">Firebase</div>
						<div class="skill-item">Cloudflare</div>
						<div class="skill-item">Netlify</div>
						<div class="skill-item">Vercel</div>
						<div class="skill-item">MySQL</div>
						<div class="skill-item">Docker</div>
						<div class="skill-item">Git</div>
						<div class="skill-item">GitHub</div>
						<div class="skill-item">Raspberry Pi</div>
						<div class="skill-item">Adobe Photoshop</div>
						<div class="skill-item">Adobe Illustrator</div>
						<div class="skill-item">Adobe Premiere Pro</div>
						<div class="skill-item">Figma</div>
						<div class="skill-item">Blender</div>
						<div class="skill-item">Canva</div>
						<div class="skill-item">GIMP</div>
						<div class="skill-item">Unity</div>
						<div class="skill-item">Markdown</div>
						<div class="skill-item">TOR</div>
						<div class="skill-item">PlayStation Network</div>
						<div class="skill-item">NVIDIA</div>
						<div class="skill-item">Steam</div>
						<div class="skill-item">Ubisoft</div>
						<div class="skill-item">PCB Design</div>
						<div class="skill-item">3D Modeling</div>
					</div>
				</div>
			</div>
		</div>
	</section>

	<section id="projects" class="projects">
		<div class="container">
			<h2>Some Things I Work On</h2>
			<div class="project-grid">
				<div class="project-card">
					<div class="project-image">
						<div class="placeholder-image">🥩</div>
					</div>
					<div class="project-content">
						<h3>BBQR (Barbequer)</h3>
						<p>
							A BBQ-themed terminal QR code generator that "grills your data to
							perfection"! Features cross-platform WiFi QR codes, file
							upload/sharing, and multi-threaded operations.
						</p>
						<div class="project-tech">
							<span class="tech-tag">Python</span>
							<span class="tech-tag">QR Codes</span>
							<span class="tech-tag">CLI</span>
						</div>
					</div>
				</div>

				<div class="project-card">
					<div class="project-image">
						<div class="placeholder-image">⌨️</div>
					</div>
					<div class="project-content">
						<h3>TerminalWriter</h3>
						<p>
							A terminal-based text editor with modern features like syntax
							highlighting, multiple tabs, and customizable themes. Designed for
							developers who prefer working in the terminal environment with
							efficient keyboard shortcuts.
						</p>
						<div class="project-tech">
							<span class="tech-tag">Python</span>
							<span class="tech-tag">Terminal UI</span>
							<span class="tech-tag">Text Editor</span>
						</div>
					</div>
				</div>

				<div class="project-card">
					<div class="project-image">
						<div class="placeholder-image">⭐</div>
					</div>
					<div class="project-content">
						<h3>Life Points</h3>
						<p>
							A Flutter mobile app that gamifies productivity by rewarding
							points for task completion. Features offline-first design,
							deadline tracking, and penalty systems for overdue tasks.
						</p>
						<div class="project-tech">
							<span class="tech-tag">Flutter</span>
							<span class="tech-tag">Dart</span>
							<span class="tech-tag">Mobile</span>
						</div>
					</div>
				</div>
			</div>
		</div>
	</section>

	<section class="contact">
		<div class="container">
			<h2>Let's Connect & Create Something Amazing Together!</h2>
			<p>
				Whether you have a project in mind, want to collaborate, or just want to
				chat about tech and keyboards, I'd love to hear from you!
			</p>
			<div class="contact-links">
				<a href="mailto:ishmansingh24@gmail.com" class="contact-link">
					<span class="icon">📧</span>
					Email
				</a>
				<a
					href="https://github.com/foglomon"
					class="contact-link"
					target="_blank"
					rel="noopener"
				>
					<img src="/assets/images/github.png" alt="GitHub" class="icon-img" />
					GitHub
				</a>
			</div>
		</div>
	</section>

	<footer class="footer">
		<div class="container">
			<p>&copy; 2025 Ishman Singh. Built with &hearts;.</p>
			<a href="/" class="back-link">← Back to Keyboard</a>
		</div>
	</footer>
</div>

<style>
	.portfolio-container {
		margin: 0;
		padding: 0;
		font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
			Ubuntu, Cantarell, sans-serif;
		line-height: 1.6;
		color: #333;
		opacity: 0;
		transform: translateY(20px);
		transition: all 0.8s ease;
		min-height: 100vh;
		overflow-x: hidden;
	}

	/* Hide scrollbar but allow scrolling */
	:global(html),
	:global(body) {
		scrollbar-width: none; /* Firefox */
		-ms-overflow-style: none; /* Internet Explorer 10+ */
	}

	:global(html::-webkit-scrollbar),
	:global(body::-webkit-scrollbar) {
		display: none; /* WebKit */
	}

	.portfolio-container.mounted {
		opacity: 1;
		transform: translateY(0);
	}

	.container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 20px;
	}

	.hero {
		background: url(assets/images/hero-bg.jpg);
		background-size: cover;
		background-position: center;
		background-repeat: no-repeat;
		color: white;
		min-height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		text-align: center;
		position: relative;
	}

	.hero-content {
		width: 100%;
		max-width: 800px;
		padding: 0 20px;
	}

	.image-disclaimer {
		position: absolute;
		bottom: 20px;
		right: 20px;
		font-size: 0.75rem;
		opacity: 0.7;
		background: rgba(0, 0, 0, 0.3);
		padding: 4px 8px;
		border-radius: 4px;
		backdrop-filter: blur(4px);
	}

	.name {
		font-size: 4rem;
		font-weight: 700;
		margin: 0 0 1rem 0;
		color: #fff;
		text-shadow:
			2px 2px 8px rgba(0, 0, 0, 0.8),
			0 0 20px rgba(0, 0, 0, 0.5);
		filter: drop-shadow(0 4px 12px rgba(0, 0, 0, 0.6));
	}

	.tagline {
		font-size: 1.5rem;
		margin: 0 0 2rem 0;
		color: #fff;
		text-shadow:
			1px 1px 6px rgba(0, 0, 0, 0.8),
			0 0 15px rgba(0, 0, 0, 0.4);
	}

	.hero-actions {
		display: flex;
		gap: 1rem;
		justify-content: center;
		flex-wrap: wrap;
	}

	.btn {
		padding: 12px 24px;
		border-radius: 6px;
		text-decoration: none;
		font-weight: 500;
		transition: all 0.3s ease;
		display: inline-block;
		font-family: inherit;
		border: none;
		cursor: pointer;
		user-select: none;
	}

	.btn.primary {
		background: rgba(255, 255, 255, 0.95);
		color: #2c3e50;
		font-weight: 600;
		box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
		backdrop-filter: blur(10px);
		border: 2px solid rgba(255, 255, 255, 0.8);
	}

	.btn.primary:hover {
		transform: translateY(-2px);
		box-shadow: 0 8px 25px rgba(0, 0, 0, 0.4);
		background: rgba(255, 255, 255, 1);
	}

	.btn.secondary {
		background: rgba(255, 255, 255, 0.15);
		color: #fff;
		border: 2px solid rgba(255, 255, 255, 0.8);
		font-weight: 600;
		text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.5);
		box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
		backdrop-filter: blur(10px);
	}

	.btn.secondary:hover {
		background: rgba(255, 255, 255, 0.95);
		color: #2c3e50;
		text-shadow: none;
		transform: translateY(-2px);
		box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
	}

	.about {
		padding: 80px 0;
		background: #f8f9fa;
	}

	.about h2 {
		text-align: center;
		font-size: 2.5rem;
		margin-bottom: 3rem;
		color: #2c3e50;
	}

	.about-content {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 4rem;
		align-items: start;
	}

	.about-text p {
		font-size: 1.1rem;
		margin-bottom: 1.5rem;
		color: #555;
	}

	.skills h3 {
		margin-bottom: 1.5rem;
		color: #2c3e50;
	}

	.skill-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
		gap: 0.5rem;
	}

	.skill-item {
		background: #667eea;
		color: white;
		padding: 8px 12px;
		border-radius: 20px;
		text-align: center;
		font-size: 0.9rem;
		font-weight: 500;
	}

	.projects {
		padding: 80px 0;
		background: #fff;
	}

	.projects h2 {
		text-align: center;
		font-size: 2.5rem;
		margin-bottom: 3rem;
		color: #2c3e50;
	}

	.project-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
		gap: 2rem;
	}

	.project-card {
		background: #fff;
		border-radius: 12px;
		overflow: hidden;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
		transition:
			transform 0.3s ease,
			box-shadow 0.3s ease;
	}

	.project-card:hover {
		transform: translateY(-5px);
		box-shadow: 0 8px 30px rgba(0, 0, 0, 0.15);
	}

	.project-image {
		height: 200px;
		background: linear-gradient(135deg, #667eea, #764ba2);
		display: flex;
		align-items: center;
		justify-content: center;
	}

	/* Unique gradients for each project */
	.project-card:nth-child(1) .project-image {
		background: linear-gradient(135deg, #ff6b6b, #ff5722);
	}

	.project-card:nth-child(2) .project-image {
		background: linear-gradient(135deg, #4ecdc4, #26a69a);
	}

	.project-card:nth-child(3) .project-image {
		background: linear-gradient(135deg, #feca57, #ff9ff3);
	}

	.placeholder-image {
		font-size: 4rem;
	}

	.project-content {
		padding: 1.5rem;
	}

	.project-content h3 {
		margin: 0 0 1rem 0;
		color: #2c3e50;
	}

	.project-content p {
		color: #666;
		margin-bottom: 1rem;
	}

	.project-tech {
		display: flex;
		flex-wrap: wrap;
		gap: 0.5rem;
	}

	.tech-tag {
		background: #e9ecef;
		color: #495057;
		padding: 4px 8px;
		border-radius: 12px;
		font-size: 0.8rem;
		font-weight: 500;
	}

	.contact {
		padding: 80px 0;
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		color: white;
		text-align: center;
		position: relative;
		overflow: hidden;
	}

	.contact::before {
		content: "";
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><defs><pattern id="grain" width="100" height="100" patternUnits="userSpaceOnUse"><circle cx="25" cy="25" r="1" fill="white" opacity="0.1"/><circle cx="75" cy="75" r="1" fill="white" opacity="0.1"/><circle cx="50" cy="10" r="0.5" fill="white" opacity="0.1"/><circle cx="90" cy="40" r="0.5" fill="white" opacity="0.1"/><circle cx="10" cy="80" r="0.5" fill="white" opacity="0.1"/></pattern></defs><rect width="100" height="100" fill="url(%23grain)"/></svg>');
		pointer-events: none;
	}

	.contact h2 {
		font-size: 2.5rem;
		margin-bottom: 1rem;
		position: relative;
		z-index: 1;
	}

	.contact p {
		font-size: 1.2rem;
		margin-bottom: 2rem;
		opacity: 0.9;
		position: relative;
		z-index: 1;
	}

	.contact-links {
		display: flex;
		justify-content: center;
		gap: 2rem;
		flex-wrap: wrap;
		position: relative;
		z-index: 1;
	}

	.contact-link {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		padding: 12px 24px;
		background: rgba(255, 255, 255, 0.2);
		color: white;
		text-decoration: none;
		border-radius: 6px;
		transition: all 0.3s ease;
		font-family: inherit;
		user-select: none;
		backdrop-filter: blur(10px);
		border: 1px solid rgba(255, 255, 255, 0.3);
	}

	.contact-link:hover {
		background: rgba(255, 255, 255, 0.9);
		color: #667eea;
		transform: translateY(-2px);
		box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
	}

	.icon {
		font-size: 1.2rem;
	}

	.icon-img {
		height: 1.2rem;
		width: auto;
		filter: brightness(0) invert(1);
		object-fit: contain;
	}

	.footer {
		background: #1a1a1a;
		color: white;
		padding: 2rem 0;
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
		font-family: inherit;
		user-select: none;
	}

	.back-link:hover {
		color: #764ba2;
	}

	/* Responsive Design */
	@media (max-width: 768px) {
		.name {
			font-size: 2.5rem;
		}

		.tagline {
			font-size: 1.2rem;
		}

		.about-content {
			grid-template-columns: 1fr;
			gap: 2rem;
		}

		.project-grid {
			grid-template-columns: 1fr;
		}

		.contact-links {
			flex-direction: column;
			align-items: center;
		}

		.footer .container {
			flex-direction: column;
			text-align: center;
		}
	}

	* {
		box-sizing: border-box;
	}

	/* Prevent font loading issues */
	.portfolio-container * {
		font-family: inherit;
	}
</style>
