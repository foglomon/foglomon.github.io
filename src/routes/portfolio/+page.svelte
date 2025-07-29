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
			<h1 class="name">foglomon</h1>
			<p class="tagline">Full Stack Developer & UI/UX Designer</p>
			<div class="hero-actions">
				<a href="#about" class="btn primary">About Me</a>
				<a href="#projects" class="btn secondary">View Projects</a>
			</div>
		</div>
	</header>

	<section id="about" class="about">
		<div class="container">
			<h2>About Me</h2>
			<div class="about-content">
				<div class="about-text">
					<p>
						Welcome to my digital workspace! I'm a passionate developer who
						loves creating innovative solutions and beautiful user experiences.
						My journey in tech has taken me through various domains, from web
						development to system architecture.
					</p>
					<p>
						I specialize in modern web technologies, with expertise in
						JavaScript/TypeScript, React, Svelte, Node.js, and cloud platforms.
						I believe in writing clean, maintainable code and creating intuitive
						user interfaces.
					</p>
				</div>
				<div class="skills">
					<h3>Skills & Technologies</h3>
					<div class="skill-grid">
						<div class="skill-item">JavaScript/TypeScript</div>
						<div class="skill-item">React/Svelte</div>
						<div class="skill-item">Node.js</div>
						<div class="skill-item">Python</div>
						<div class="skill-item">Docker</div>
						<div class="skill-item">AWS/GCP</div>
						<div class="skill-item">PostgreSQL</div>
						<div class="skill-item">Git/CI/CD</div>
					</div>
				</div>
			</div>
		</div>
	</section>

	<section id="projects" class="projects">
		<div class="container">
			<h2>Featured Projects</h2>
			<div class="project-grid">
				<div class="project-card">
					<div class="project-image">
						<div class="placeholder-image">🚀</div>
					</div>
					<div class="project-content">
						<h3>Project Alpha</h3>
						<p>
							A modern web application built with SvelteKit and TypeScript,
							featuring real-time collaboration and advanced data visualization.
						</p>
						<div class="project-tech">
							<span class="tech-tag">SvelteKit</span>
							<span class="tech-tag">TypeScript</span>
							<span class="tech-tag">WebSocket</span>
						</div>
					</div>
				</div>

				<div class="project-card">
					<div class="project-image">
						<div class="placeholder-image">⚡</div>
					</div>
					<div class="project-content">
						<h3>Lightning API</h3>
						<p>
							High-performance REST API with microservices architecture,
							handling millions of requests per day with 99.9% uptime.
						</p>
						<div class="project-tech">
							<span class="tech-tag">Node.js</span>
							<span class="tech-tag">Docker</span>
							<span class="tech-tag">Redis</span>
						</div>
					</div>
				</div>

				<div class="project-card">
					<div class="project-image">
						<div class="placeholder-image">🎨</div>
					</div>
					<div class="project-content">
						<h3>Design System</h3>
						<p>
							Comprehensive design system and component library used across
							multiple products, ensuring consistency and accessibility.
						</p>
						<div class="project-tech">
							<span class="tech-tag">React</span>
							<span class="tech-tag">Storybook</span>
							<span class="tech-tag">Figma</span>
						</div>
					</div>
				</div>
			</div>
		</div>
	</section>

	<section class="contact">
		<div class="container">
			<h2>Let's Connect</h2>
			<p>Interested in working together? I'd love to hear from you!</p>
			<div class="contact-links">
				<a href="mailto:hello@foglomon.dev" class="contact-link">
					<span class="icon">📧</span>
					Email
				</a>
				<a
					href="https://github.com/foglomon"
					class="contact-link"
					target="_blank"
					rel="noopener"
				>
					<span class="icon">💻</span>
					GitHub
				</a>
				<a
					href="https://linkedin.com/in/foglomon"
					class="contact-link"
					target="_blank"
					rel="noopener"
				>
					<span class="icon">💼</span>
					LinkedIn
				</a>
			</div>
		</div>
	</section>

	<footer class="footer">
		<div class="container">
			<p>&copy; 2025 foglomon. Built with SvelteKit.</p>
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
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		color: white;
		min-height: 100vh;
		display: flex;
		align-items: center;
		text-align: center;
	}

	.hero-content {
		width: 100%;
	}

	.name {
		font-size: 4rem;
		font-weight: 700;
		margin: 0 0 1rem 0;
		background: linear-gradient(45deg, #fff, #f0f0f0);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.tagline {
		font-size: 1.5rem;
		margin: 0 0 2rem 0;
		opacity: 0.9;
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
		background: #fff;
		color: #667eea;
	}

	.btn.primary:hover {
		transform: translateY(-2px);
		box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
	}

	.btn.secondary {
		background: transparent;
		color: #fff;
		border: 2px solid #fff;
	}

	.btn.secondary:hover {
		background: #fff;
		color: #667eea;
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
		background: #2c3e50;
		color: white;
		text-align: center;
	}

	.contact h2 {
		font-size: 2.5rem;
		margin-bottom: 1rem;
	}

	.contact p {
		font-size: 1.2rem;
		margin-bottom: 2rem;
		opacity: 0.9;
	}

	.contact-links {
		display: flex;
		justify-content: center;
		gap: 2rem;
		flex-wrap: wrap;
	}

	.contact-link {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		padding: 12px 24px;
		background: #34495e;
		color: white;
		text-decoration: none;
		border-radius: 6px;
		transition: all 0.3s ease;
		font-family: inherit;
		user-select: none;
	}

	.contact-link:hover {
		background: #667eea;
		transform: translateY(-2px);
	}

	.icon {
		font-size: 1.2rem;
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

	/* Ensure smooth scrolling and prevent layout issues */
	html {
		scroll-behavior: smooth;
	}

	* {
		box-sizing: border-box;
	}

	/* Prevent font loading issues */
	.portfolio-container * {
		font-family: inherit;
	}
</style>
