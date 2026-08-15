<script lang="ts">
	let hovered = $state<string | null>(null);
</script>

<svelte:head>
	<title>ANIHIE — Mechanical Engineering</title>
	<meta
		name="description"
		content="ANIHIE — Mechanical Engineering Portfolio"
	/>
</svelte:head>

<div class="home">
	<!-- YOUR ACTUAL BACKGROUND IMAGE -->
	<div class="background"></div>

	<!-- Very subtle readability layer -->
	<div class="overlay"></div>

	<!-- Navigation -->
	<nav class="navigation" aria-label="Main navigation">
		<a
			href="/about"
			class:active={hovered === 'about'}
			onmouseenter={() => (hovered = 'about')}
			onmouseleave={() => (hovered = null)}
		>
			<span class="dot"></span>
			<span class="label">ABOUT</span>
		</a>

		<a
			href="/projects"
			class:active={hovered === 'projects'}
			onmouseenter={() => (hovered = 'projects')}
			onmouseleave={() => (hovered = null)}
		>
			<span class="dot"></span>
			<span class="label">PROJECTS</span>
		</a>

		<a
			href="/contact"
			class:active={hovered === 'contact'}
			onmouseenter={() => (hovered = 'contact')}
			onmouseleave={() => (hovered = null)}
		>
			<span class="dot"></span>
			<span class="label">CONTACT</span>
		</a>
	</nav>

	<!-- Navigation line -->
	<div class="nav-line"></div>
</div>

<style>
	:global(html),
	:global(body) {
		margin: 0;
		padding: 0;
		width: 100%;
		height: 100%;

		background: #000;
	}

	:global(body) {
		overflow: hidden;
	}

	:global(*) {
		box-sizing: border-box;
	}

	.home {
		position: relative;

		width: 100vw;
		height: 100vh;

		min-height: 650px;

		overflow: hidden;

		background: #000;
	}

	/* =========================================================
	   BACKGROUND
	   ========================================================= */

				.background {
					position: absolute;
					inset: 0;

					background-image: url('/home-background.png');

					background-repeat: no-repeat;

					/* Show the ENTIRE source image */
					background-position: center center;
					background-size: contain;

					background-color: #000;

					filter:
						brightness(0.95)
						contrast(1.05);
				}

				.overlay {
					position: absolute;
					inset: 0;

					pointer-events: none;

					background: rgba(0, 0, 0, 0.08);
				}

	/* =========================================================
	   NAVIGATION
	   ========================================================= */

	.navigation {
		position: absolute;
		z-index: 20;

		left: 32.2%;
		top: 81.2%;

		width: 35%;

		display: grid;
		grid-template-columns: repeat(3, 1fr);

		align-items: start;
	}

	.navigation a {
		position: relative;

		display: flex;
		flex-direction: column;
		align-items: center;

		color: #e50000;

		text-decoration: none;

		font-family:
			Arial Narrow,
			Arial,
			Helvetica,
			sans-serif;

		transition:
			transform 0.18s ease,
			filter 0.18s ease;
	}

	.navigation a:hover,
	.navigation a.active {
		transform: translateY(-3px);

		filter:
			drop-shadow(0 0 5px rgba(255, 0, 0, 0.75))
			drop-shadow(0 0 14px rgba(255, 0, 0, 0.35));
	}

	.dot {
		display: block;

		width: 10px;
		height: 10px;

		margin-bottom: 17px;

		border-radius: 50%;

		background: #e50000;

		box-shadow:
			0 0 5px rgba(255, 0, 0, 0.7),
			0 0 14px rgba(255, 0, 0, 0.25);
	}

	.label {
		font-size: clamp(15px, 1.35vw, 24px);

		font-weight: 700;

		letter-spacing: 0.01em;

		text-shadow:
			0 2px 5px rgba(0, 0, 0, 0.95);
	}

	.nav-line {
		position: absolute;
		z-index: 15;

		left: 32.25%;
		top: 81.1%;

		width: 35%;
		height: 2px;

		background: #e50000;

		box-shadow:
			0 0 6px rgba(255, 0, 0, 0.3);
	}

	@media (max-width: 900px) {
		.navigation {
			left: 25%;
			width: 50%;
			top: 76%;
		}

		.nav-line {
			left: 25%;
			width: 50%;
			top: 75.9%;
		}

		.label {
			font-size: 14px;
		}
	}

	@media (max-width: 600px) {
		.background {
			background-position: 48% center;
		}

		.navigation {
			left: 18%;
			width: 64%;
			top: 76%;
		}

		.nav-line {
			left: 18%;
			width: 64%;
			top: 75.9%;
		}

		.label {
			font-size: 11px;
		}

		.dot {
			width: 7px;
			height: 7px;
			margin-bottom: 9px;
		}
	}
</style>
