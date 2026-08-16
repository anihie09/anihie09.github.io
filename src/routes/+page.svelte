<script lang="ts">
	import InteractiveAscii from '$lib/InteractiveAscii.svelte';
</script>

<svelte:head>
	<title>ANIHIE — Mechanical Engineering</title>

	<meta
		name="description"
		content="ANIHIE — Mechanical Engineering Portfolio"
	/>
</svelte:head>

<div class="home">
	<!-- =================================================
	     BACKGROUND
	     ================================================= -->

	<div class="background"></div>

	<div class="background-shade"></div>

	<!-- Interactive mouse / halftone layer -->
	<InteractiveAscii />

	<!-- =================================================
	     PAGE INTERFACE
	     ================================================= -->

	<div class="interface">
		<!-- =============================================
		     SYSTEM INFORMATION
		     ============================================= -->

		<div class="system-info">
			<div class="system-heading">
				SYSTEMS UPDATING<span class="loading-dots">
					<span>.</span><span>.</span><span>.</span>
				</span>
			</div>

			<div>
				University of Southern California
			</div>

			<div>
				Mechanical Engineering
			</div>

			<div>
				Vietnamese American
			</div>
		</div>

		<!-- =============================================
		     PORTRAIT TARGET FRAME
		     ============================================= -->

		<div class="portrait-frame">
			<div class="portrait-label">
				ANI_LE v1
			</div>

			<!-- White technical corner markers -->

			<div class="portrait-corner top-left"></div>
			<div class="portrait-corner top-right"></div>
			<div class="portrait-corner bottom-left"></div>
			<div class="portrait-corner bottom-right"></div>

			<!-- Tiny interface details -->

			<div class="frame-index frame-index-left">
				01
			</div>

			<div class="frame-index frame-index-right">
				09
			</div>
		</div>

		<!-- =============================================
		     NAVIGATION
		     ============================================= -->

		<nav
			class="navigation"
			aria-label="Main navigation"
		>
			<a href="/about">
				<span class="arrow">
					&gt;
				</span>

				<span>
					ABOUT
				</span>
			</a>

			<a href="/projects">
				<span class="arrow">
					&gt;
				</span>

				<span>
					PROJECTS
				</span>
			</a>

			<a href="/contact">
				<span class="arrow">
					&gt;
				</span>

				<span>
					CONTACT
				</span>
			</a>
		</nav>
	</div>
</div>

<style>
	/* =====================================================
	   SHARED INTERFACE COLOR
	   ===================================================== */

	:global(:root) {
		--hud-pink: #ff0080;

		--hud-pink-soft:
			rgba(
				255,
				0,
				128,
				0.55
			);

		--hud-pink-faint:
			rgba(
				255,
				0,
				128,
				0.2
			);
	}

	/* =====================================================
	   GLOBAL
	   ===================================================== */

	:global(html),
	:global(body) {
		margin: 0;
		padding: 0;

		width: 100%;
		min-height: 100%;

		background: #000;
	}

	:global(body) {
		overflow: hidden;
	}

	:global(*) {
		box-sizing: border-box;
	}

	/* =====================================================
	   HOME
	   ===================================================== */

	.home {
		position: relative;

		width: 100vw;
		height: 100vh;

		min-height: 650px;

		overflow: hidden;

		background: #000;
	}

	/* =====================================================
	   BACKGROUND
	   ===================================================== */

	.background {
		position: absolute;

		inset: 0;

		z-index: 0;

		background-image:
			url('/home-background.png');

		background-repeat:
			no-repeat;

		background-position:
			center center;

		background-size:
			cover;

		background-color:
			#000;
	}

	.background-shade {
		position: absolute;

		inset: 0;

		z-index: 1;

		pointer-events: none;

		background:
			linear-gradient(
				90deg,
				rgba(
					0,
					0,
					0,
					0.05
				)
				0%,
				transparent
				40%,
				rgba(
					0,
					0,
					0,
					0.08
				)
				72%,
				rgba(
					0,
					0,
					0,
					0.3
				)
				100%
			),
			linear-gradient(
				180deg,
				rgba(
					0,
					0,
					0,
					0.06
				)
				0%,
				transparent
				58%,
				rgba(
					0,
					0,
					0,
					0.15
				)
				100%
			);
	}

	/* =====================================================
	   INTERFACE
	   ===================================================== */

	.interface {
		position: relative;

		z-index: 10;

		width: 100%;
		height: 100%;

		pointer-events: none;
	}

	/* =====================================================
	   SYSTEM INFORMATION
	   ===================================================== */

	.system-info {
		position: absolute;

		top: 5.3%;
		left: 2.8%;

		color:
			rgba(
				255,
				255,
				255,
				0.92
			);

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size:
			clamp(
				10px,
				0.78vw,
				14px
			);

		line-height: 1.55;

		letter-spacing:
			0.01em;

		text-shadow:
			0 2px 5px
			rgba(
				0,
				0,
				0,
				0.95
			);

		animation:
			system-idle
			4.7s
			steps(2, end)
			infinite;
	}

	.system-heading {
		white-space: nowrap;
	}

	.loading-dots span {
		display: inline-block;

		animation:
			dot-blink
			1.35s
			steps(1, end)
			infinite;
	}

	.loading-dots span:nth-child(2) {
		animation-delay:
			0.18s;
	}

	.loading-dots span:nth-child(3) {
		animation-delay:
			0.36s;
	}

	/* =====================================================
	   PORTRAIT FRAME

	   IMPORTANT:
	   Same pink and same 1px line weight as the HUD.
	   ===================================================== */

	.portrait-frame {
		position: absolute;

		left: 49%;
		top: 18.5%;

		width: 28.5%;
		height: 49.5%;

		/*
		 * This is intentionally 1px now.
		 *
		 * It matches the outer technical HUD instead
		 * of looking like a separate heavy box.
		 */
		border:
			1px solid
			var(--hud-pink);

		/*
		 * No heavy red/pink glow.
		 *
		 * Just a tiny technical signal presence.
		 */
		box-shadow:
			0 0 3px
			rgba(
				255,
				0,
				128,
				0.08
			);

		pointer-events: none;
	}

	/* =====================================================
	   PORTRAIT LABEL
	   ===================================================== */

	.portrait-label {
		position: absolute;

		left: -1px;
		top: -48px;

		min-width: 140px;
		height: 48px;

		display: flex;

		align-items: center;

		padding:
			0 17px;

		/*
		 * Same exact interface pink.
		 */
		background:
			var(--hud-pink);

		color: #fff;

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size:
			clamp(
				13px,
				1vw,
				18px
			);

		letter-spacing:
			0.04em;

		/*
		 * Thin technical edge.
		 */
		border:
			1px solid
			var(--hud-pink);

		border-bottom: 0;
	}

	/* =====================================================
	   PORTRAIT CORNERS
	   ===================================================== */

	.portrait-corner {
		position: absolute;

		width: 13px;
		height: 13px;

		border-color:
			rgba(
				255,
				255,
				255,
				0.88
			);
	}

	.portrait-corner.top-left {
		left: -1px;
		top: -1px;

		border-left:
			1px solid;

		border-top:
			1px solid;
	}

	.portrait-corner.top-right {
		right: -1px;
		top: -1px;

		border-right:
			1px solid;

		border-top:
			1px solid;
	}

	.portrait-corner.bottom-left {
		left: -1px;
		bottom: -1px;

		border-left:
			1px solid;

		border-bottom:
			1px solid;
	}

	.portrait-corner.bottom-right {
		right: -1px;
		bottom: -1px;

		border-right:
			1px solid;

		border-bottom:
			1px solid;
	}

	/* =====================================================
	   SMALL FRAME INDEXES
	   ===================================================== */

	.frame-index {
		position: absolute;

		bottom: 8px;

		color:
			rgba(
				255,
				0,
				128,
				0.42
			);

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size: 5px;

		letter-spacing:
			0.12em;
	}

	.frame-index-left {
		left: 8px;
	}

	.frame-index-right {
		right: 8px;
	}

	/* =====================================================
	   NAVIGATION
	   ===================================================== */

	.navigation {
		position: absolute;

		/*
		 * Exact same left edge as portrait box.
		 */
		left: 49%;

		top: 69.5%;

		display: flex;

		flex-direction: column;

		align-items: flex-start;

		gap: 11px;

		pointer-events: auto;
	}

	.navigation a {
		position: relative;

		display: flex;

		align-items: center;

		min-height: 20px;

		color:
			rgba(
				255,
				255,
				255,
				0.92
			);

		text-decoration: none;

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size:
			clamp(
				10px,
				0.82vw,
				14px
			);

		font-weight: 600;

		letter-spacing:
			0.015em;

		text-shadow:
			0 2px 5px
			rgba(
				0,
				0,
				0,
				0.95
			);
	}

	.navigation .arrow {
		width: 0;

		overflow: hidden;

		color:
			var(--hud-pink);

		opacity: 0;

		transform:
			translateX(-5px);

		transition:
			width
				120ms
				ease,
			opacity
				80ms
				linear,
			transform
				120ms
				ease;
	}

	.navigation a:hover,
	.navigation a:focus-visible {
		color:
			var(--hud-pink);

		outline: none;

		animation:
			nav-glitch
			260ms
			steps(2, end)
			1;
	}

	.navigation a:hover .arrow,
	.navigation a:focus-visible .arrow {
		width: 16px;

		opacity: 1;

		transform:
			translateX(0);
	}

	/* =====================================================
	   SYSTEM ANIMATION
	   ===================================================== */

	@keyframes system-idle {
		0%,
		82%,
		100% {
			opacity: 0.92;

			transform:
				translate(
					0,
					0
				);

			text-shadow:
				0 2px 5px
				rgba(
					0,
					0,
					0,
					0.95
				);
		}

		84% {
			opacity: 0.68;

			transform:
				translateX(-1px);

			text-shadow:
				2px 0
				rgba(
					255,
					0,
					128,
					0.32
				);
		}

		86% {
			opacity: 1;

			transform:
				translateX(2px);

			text-shadow:
				-2px 0
				rgba(
					255,
					0,
					128,
					0.35
				);
		}

		88% {
			opacity: 0.76;

			transform:
				translate(
					-1px,
					1px
				);
		}

		90% {
			opacity: 0.94;

			transform:
				translate(
					0,
					0
				);
		}
	}

	@keyframes dot-blink {
		0%,
		24% {
			opacity: 0.15;
		}

		25%,
		100% {
			opacity: 1;
		}
	}

	/* =====================================================
	   NAVIGATION GLITCH
	   ===================================================== */

	@keyframes nav-glitch {
		0% {
			transform:
				translate(
					0,
					0
				);
		}

		20% {
			transform:
				translate(
					-1px,
					0
				);

			text-shadow:
				2px 0
				rgba(
					255,
					0,
					128,
					0.7
				);
		}

		40% {
			transform:
				translate(
					2px,
					-1px
				);

			text-shadow:
				-2px 0
				rgba(
					255,
					255,
					255,
					0.4
				);
		}

		60% {
			transform:
				translate(
					-1px,
					1px
				);
		}

		80% {
			transform:
				translate(
					1px,
					0
				);
		}

		100% {
			transform:
				translate(
					0,
					0
				);
		}
	}

	/* =====================================================
	   RESPONSIVE
	   ===================================================== */

	@media (max-width: 1100px) {
		.portrait-frame {
			left: 45%;

			width: 34%;
		}

		.navigation {
			left: 45%;
		}
	}

	@media (max-width: 800px) {
		.system-info {
			top: 60px;
			left: 28px;
		}

		.portrait-frame {
			left: 36%;

			top: 23%;

			width: 48%;
			height: 46%;
		}

		.navigation {
			left: 36%;

			top: 71%;
		}

		.portrait-label {
			height: 38px;

			top: -38px;

			min-width: 115px;

			padding:
				0 12px;

			font-size: 13px;
		}
	}

	@media (max-width: 600px) {
		.home {
			min-height: 600px;
		}

		.system-info {
			top: 50px;
			left: 22px;

			font-size: 8px;
		}

		.portrait-frame {
			left: 22%;

			top: 27%;

			width: 62%;
			height: 40%;
		}

		.navigation {
			left: 22%;

			top: 70%;
		}

		.navigation a {
			font-size: 10px;
		}

		.portrait-label {
			height: 32px;

			top: -32px;

			min-width: 100px;

			font-size: 11px;
		}
	}
</style>
