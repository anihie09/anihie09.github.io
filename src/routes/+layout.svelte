<script lang="ts">
	import { onMount } from 'svelte';
	import {
		afterNavigate,
		onNavigate
	} from '$app/navigation';
	import type { Snippet } from 'svelte';

	import CursorHUD from '$lib/CursorHUD.svelte';

	let { children }: { children: Snippet } = $props();

	let booting = $state(true);
	let bootExiting = $state(false);

	let transitioning = $state(false);
	let transitionLeaving = $state(false);

	let pendingPageTransition = false;

	/* =====================================================
	   INITIAL BOOT
	   ===================================================== */

	onMount(() => {
		const timer = window.setTimeout(() => {
			bootExiting = true;

			window.setTimeout(() => {
				booting = false;
				bootExiting = false;
			}, 1000);
		}, 2200);

		return () => {
			window.clearTimeout(timer);
		};
	});

	/* =====================================================
	   PAGE TRANSITION
	   ===================================================== */

	onNavigate((navigation) => {
		/*
		 * Don't run the route transition for the initial
		 * page load. The boot animation handles that.
		 */
		if (!navigation.from) {
			return;
		}

		pendingPageTransition = true;

		transitioning = true;
		transitionLeaving = false;

		/*
		 * Hold navigation very briefly so the first half
		 * of the glitch can cover the old page.
		 */
		return new Promise<void>((resolve) => {
			window.setTimeout(() => {
				resolve();
			}, 430);
		});
	});

	afterNavigate(() => {
		if (!pendingPageTransition) {
			return;
		}

		pendingPageTransition = false;

		window.setTimeout(() => {
			transitionLeaving = true;

			window.setTimeout(() => {
				transitioning = false;
				transitionLeaving = false;
			}, 720);
		}, 60);
	});
</script>

<svelte:head>
	<meta
		name="theme-color"
		content="#000000"
	/>
</svelte:head>

<div class="site">

	<!-- =================================================
	     GLOBAL CURSOR SYSTEM

	     KEEP THIS HERE.

	     Because this is +layout.svelte, CursorHUD is now
	     mounted ONCE and persists across:
	     /
	     /about
	     /projects
	     /contact
	     /skills
	     and future routes.

	     CursorHUD provides:
	     --cursor-x
	     --cursor-y
	     --cursor-intensity
	     --cursor-vx
	     --cursor-vy
	     ================================================= -->

	<CursorHUD />

	<!-- =================================================
	     GLOBAL HOME / SIGNATURE BUTTON
	     ================================================= -->

	<a
		class="home-button"
		href="/"
		aria-label="Return home"
		title="Home"
	>
		<span class="signature-base">
			<img
				src="/signature.png"
				alt="Ani"
			/>
		</span>

		<span
			class="signature-glitch signature-glitch-a"
			aria-hidden="true"
		>
			<img
				src="/signature.png"
				alt=""
			/>
		</span>

		<span
			class="signature-glitch signature-glitch-b"
			aria-hidden="true"
		>
			<img
				src="/signature.png"
				alt=""
			/>
		</span>

		<span
			class="signature-scan"
			aria-hidden="true"
		></span>
	</a>

	<!-- =================================================
	     ALL ROUTES RENDER HERE

	     Home, About, Projects, Contact, etc.
	     ================================================= -->

	<div class="route-content">
		{@render children()}
	</div>

	<!-- =================================================
	     GLOBAL CYBER UI BORDER
	     ================================================= -->

	<div
		class="global-hud"
		aria-hidden="true"
	>
		<!-- MAIN BORDER -->

		<div class="hud-border hud-top"></div>
		<div class="hud-border hud-right"></div>
		<div class="hud-border hud-bottom"></div>
		<div class="hud-border hud-left"></div>

		<!-- TOP CENTER -->

		<div class="top-system">
			<div class="top-line"></div>

			<div class="top-node node-a"></div>
			<div class="top-node node-b"></div>
			<div class="top-node node-c"></div>

			<div class="top-bars">
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
			</div>
		</div>

		<!-- TOP RIGHT -->

		<div class="system-module">
			<div class="system-label">
				SYS
			</div>

			<div class="system-bars">
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
			</div>

			<div class="system-status">
				01 / ONLINE
			</div>
		</div>

		<!-- LEFT RADAR -->

		<div class="radar radar-left">
			<div class="radar-ring radar-ring-a"></div>
			<div class="radar-ring radar-ring-b"></div>
			<div class="radar-ring radar-ring-c"></div>

			<div class="radar-horizontal"></div>
			<div class="radar-vertical"></div>

			<div class="radar-sweep"></div>

			<div class="radar-dot"></div>
		</div>

		<!-- RIGHT RADAR -->

		<div class="radar radar-right">
			<div class="radar-ring radar-ring-a"></div>
			<div class="radar-ring radar-ring-b"></div>
			<div class="radar-ring radar-ring-c"></div>

			<div class="radar-horizontal"></div>
			<div class="radar-vertical"></div>

			<div class="radar-sweep"></div>

			<div class="radar-dot"></div>
		</div>

		<!-- LEFT DATA RAIL -->

		<div class="data-rail data-left">
			<div class="rail-number">
				03
			</div>

			<div class="rail-line"></div>

			<span class="rail-dot"></span>
			<span class="rail-dot"></span>
			<span class="rail-dot"></span>
			<span class="rail-dot"></span>

			<div class="rail-code">
				<span>021</span>
				<span>442</span>
				<span>009</span>
			</div>
		</div>

		<!-- RIGHT DATA RAIL -->

		<div class="data-rail data-right">
			<div class="rail-number">
				08
			</div>

			<div class="rail-line"></div>

			<span class="rail-dot"></span>
			<span class="rail-dot"></span>
			<span class="rail-dot"></span>
			<span class="rail-dot"></span>

			<div class="rail-code">
				<span>042</span>
				<span>117</span>
				<span>903</span>
			</div>
		</div>

		<!-- BOTTOM LEFT -->

		<div class="bottom-module bottom-left-module">
			<div class="bottom-title">
				CORE / 02
			</div>

			<div class="bottom-bars">
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
			</div>

			<div class="bottom-rule"></div>

			<div class="bottom-small">
				SYSTEM READY
			</div>
		</div>

		<!-- BOTTOM CENTER -->

		<div class="bottom-stream">
			<div class="stream-line"></div>

			<div class="stream-node stream-start"></div>
			<div class="stream-node stream-middle"></div>
			<div class="stream-node stream-end"></div>

			<div class="stream-bars">
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
			</div>

			<div class="stream-text">
				LINK ESTABLISHED
			</div>
		</div>

		<!-- BOTTOM RIGHT -->

		<div class="bottom-module bottom-right-module">
			<div class="bottom-title">
				STATUS
			</div>

			<div class="bottom-small">
				01-09 / ACTIVE
			</div>

			<div class="bottom-bars compact">
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
			</div>
		</div>

		<!-- CORNERS -->

		<div class="corner corner-tl">
			<div class="corner-long"></div>
			<div class="corner-short"></div>
			<div class="corner-dot"></div>
		</div>

		<div class="corner corner-tr">
			<div class="corner-long"></div>
			<div class="corner-short"></div>
			<div class="corner-dot"></div>
		</div>

		<div class="corner corner-bl">
			<div class="corner-long"></div>
			<div class="corner-short"></div>
			<div class="corner-dot"></div>
		</div>

		<div class="corner corner-br">
			<div class="corner-long"></div>
			<div class="corner-short"></div>
			<div class="corner-dot"></div>
		</div>
	</div>

	<!-- =================================================
	     INITIAL BOOT SCREEN
	     ================================================= -->

	{#if booting}
		<div
			class:boot-exiting={bootExiting}
			class="boot-screen"
			aria-hidden="true"
		>
			<div class="boot-grid"></div>
			<div class="boot-vignette"></div>

			<div class="boot-glitches">
				<span class="boot-glitch bg1"></span>
				<span class="boot-glitch bg2"></span>
				<span class="boot-glitch bg3"></span>
				<span class="boot-glitch bg4"></span>
				<span class="boot-glitch bg5"></span>
				<span class="boot-glitch bg6"></span>
				<span class="boot-glitch bg7"></span>
				<span class="boot-glitch bg8"></span>
			</div>

			<div class="boot-interface">
				<div class="boot-frame">
					<div class="boot-corner btl"></div>
					<div class="boot-corner btr"></div>
					<div class="boot-corner bbl"></div>
					<div class="boot-corner bbr"></div>

					<div class="boot-circle boot-circle-large"></div>
					<div class="boot-circle boot-circle-small"></div>

					<div class="boot-cross boot-cross-h"></div>
					<div class="boot-cross boot-cross-v"></div>

					<div class="boot-title">
						SYSTEM REBOOTING
					</div>

					<div class="boot-subtitle">
						HYDRA VER.01 SYS RECOVERY
					</div>

					<div class="boot-progress">
						<span></span>
						<span></span>
						<span></span>
						<span></span>
						<span></span>
						<span></span>
						<span></span>
						<span></span>
						<span></span>
						<span></span>
						<span></span>
						<span></span>
					</div>
				</div>
			</div>

			<div class="boot-flash"></div>
		</div>
	{/if}

	<!-- =================================================
	     ROUTE TRANSITION
	     ================================================= -->

	{#if transitioning}
		<div
			class:transition-leaving={transitionLeaving}
			class="route-transition"
			aria-hidden="true"
		>
			<div class="transition-black"></div>

			<div class="transition-scanlines"></div>

			<div class="transition-glitches">
				<span class="transition-strip t1"></span>
				<span class="transition-strip t2"></span>
				<span class="transition-strip t3"></span>
				<span class="transition-strip t4"></span>
				<span class="transition-strip t5"></span>
				<span class="transition-strip t6"></span>
				<span class="transition-strip t7"></span>
				<span class="transition-strip t8"></span>
				<span class="transition-strip t9"></span>
				<span class="transition-strip t10"></span>
				<span class="transition-strip t11"></span>
				<span class="transition-strip t12"></span>
			</div>

			<div class="transition-interface">
				<div class="transition-circle outer"></div>
				<div class="transition-circle inner"></div>

				<div class="transition-title">
					SYSTEM RECONFIGURE
				</div>

				<div class="transition-code">
					LINK
					<span>///</span>
					01
				</div>

				<div class="transition-progress">
					<span></span>
					<span></span>
					<span></span>
					<span></span>
					<span></span>
					<span></span>
					<span></span>
					<span></span>
				</div>
			</div>

			<div class="transition-flash"></div>
		</div>
	{/if}
</div>

<style>
	/* =====================================================
	   GLOBAL VARIABLES
	   ===================================================== */

	:global(:root) {
		--hud-pink: #ff0080;
		--hud-pink-soft: rgba(255, 0, 128, 0.55);
		--hud-pink-faint: rgba(255, 0, 128, 0.18);

		/*
		 * CursorHUD updates these globally.
		 *
		 * Individual routes can use these variables
		 * without creating another mouse listener.
		 */
		--cursor-x: 50%;
		--cursor-y: 50%;
		--cursor-intensity: 0;
		--cursor-vx: 0px;
		--cursor-vy: 0px;
	}

	/* =====================================================
	   GLOBAL RESET
	   ===================================================== */

	:global(html),
	:global(body) {
		margin: 0;
		padding: 0;

		width: 100%;
		min-width: 100%;
		min-height: 100%;

		background: #000;

		color: #fff;
	}

	:global(body) {
		min-height: 100vh;
	}

	:global(*) {
		box-sizing: border-box;
	}

	:global(a) {
		-webkit-tap-highlight-color: transparent;
	}

	/* =====================================================
	   SITE
	   ===================================================== */

	.site {
		position: relative;

		width: 100%;
		min-height: 100vh;

		background: #000;
	}

	.route-content {
		position: relative;

		z-index: 1;

		width: 100%;
		min-height: 100vh;
	}

	/* =====================================================
	   GLOBAL HOME BUTTON
	   ===================================================== */

	.home-button {
		position: fixed;

		left: 4.2%;
		top: 2.2%;

		z-index: 1000;

		width: 230px;
		height: 92px;

		display: block;

		text-decoration: none;

		cursor: pointer;

		pointer-events: auto;

		transform-origin: left top;

		transition:
			transform
			180ms
			cubic-bezier(
				0.22,
				1,
				0.36,
				1
			),
			filter
			180ms
			ease;
	}

	.signature-base,
	.signature-glitch {
		position: absolute;

		inset: 0;

		display: block;

		pointer-events: none;
	}

	.signature-base {
		z-index: 1;
	}

	.signature-glitch {
		z-index: 2;

		opacity: 0;
	}

	.signature-base img,
	.signature-glitch img {
		display: block;

		width: 100%;
		height: 100%;

		object-fit: contain;

		object-position:
			left center;
	}

	.signature-base img {
		filter:
			drop-shadow(
				0 0 4px
				rgba(
					255,
					0,
					128,
					0.2
				)
			);
	}

	.signature-glitch-a {
		filter:
			sepia(1)
			saturate(100)
			hue-rotate(285deg)
			brightness(1.5);

		mix-blend-mode: screen;
	}

	.signature-glitch-b {
		filter:
			grayscale(1)
			brightness(2.4);

		mix-blend-mode: screen;
	}

	.signature-scan {
		position: absolute;

		z-index: 3;

		left: 0;
		right: 0;

		top: 50%;

		height: 2px;

		opacity: 0;

		background:
			linear-gradient(
				90deg,
				transparent,
				var(--hud-pink),
				transparent
			);

		pointer-events: none;
	}

	.home-button:hover,
	.home-button:focus-visible {
		transform:
			translateX(3px)
			scale(1.045);

		filter:
			drop-shadow(
				0 0 5px
				rgba(
					255,
					0,
					128,
					0.7
				)
			)
			drop-shadow(
				0 0 18px
				rgba(
					255,
					0,
					128,
					0.3
				)
			);

		outline: none;
	}

	.home-button:hover
		.signature-glitch-a,
	.home-button:focus-visible
		.signature-glitch-a {
		opacity: 0.82;

		animation:
			signature-glitch-a
			470ms
			steps(7, end)
			infinite;
	}

	.home-button:hover
		.signature-glitch-b,
	.home-button:focus-visible
		.signature-glitch-b {
		opacity: 0.32;

		animation:
			signature-glitch-b
			410ms
			steps(6, end)
			infinite;
	}

	.home-button:hover
		.signature-scan,
	.home-button:focus-visible
		.signature-scan {
		animation:
			signature-scan
			820ms
			steps(5, end)
			infinite;
	}

	/* =====================================================
	   SIGNATURE GLITCH
	   ===================================================== */

	@keyframes signature-glitch-a {
		0%,
		100% {
			transform:
				translate(0, 0);

			clip-path:
				inset(0 0 0 0);
		}

		10% {
			transform:
				translate(6px, -1px);

			clip-path:
				inset(
					4% 0 80% 0
				);
		}

		22% {
			transform:
				translate(-7px, 1px);

			clip-path:
				inset(
					30% 0 48% 0
				);
		}

		35% {
			transform:
				translate(5px, 0);

			clip-path:
				inset(
					62% 0 22% 0
				);
		}

		48% {
			transform:
				translate(-5px, -1px);

			clip-path:
				inset(
					78% 0 8% 0
				);
		}

		63% {
			transform:
				translate(8px, 1px);

			clip-path:
				inset(
					15% 0 68% 0
				);
		}

		78% {
			transform:
				translate(-4px, 0);

			clip-path:
				inset(
					44% 0 36% 0
				);
		}

		91% {
			transform:
				translate(4px, -1px);

			clip-path:
				inset(
					70% 0 15% 0
				);
		}
	}

	@keyframes signature-glitch-b {
		0%,
		100% {
			transform:
				translate(0, 0);

			clip-path:
				inset(0 0 0 0);
		}

		18% {
			transform:
				translate(-4px, 0);

			clip-path:
				inset(
					17% 0 70% 0
				);
		}

		37% {
			transform:
				translate(6px, 1px);

			clip-path:
				inset(
					51% 0 27% 0
				);
		}

		56% {
			transform:
				translate(-6px, -1px);

			clip-path:
				inset(
					76% 0 9% 0
				);
		}

		75% {
			transform:
				translate(5px, 0);

			clip-path:
				inset(
					34% 0 53% 0
				);
		}
	}

	@keyframes signature-scan {
		0% {
			top: 5%;
			opacity: 0;
		}

		15% {
			opacity: 0.8;
		}

		50% {
			top: 53%;
			opacity: 1;
		}

		85% {
			top: 92%;
			opacity: 0.5;
		}

		100% {
			top: 100%;
			opacity: 0;
		}
	}

	/* =====================================================
	   GLOBAL HUD
	   ===================================================== */

	.global-hud {
		position: fixed;

		inset: 0;

		z-index: 70;

		overflow: hidden;

		pointer-events: none;

		color:
			var(--hud-pink);
	}

	/* =====================================================
	   BORDER
	   ===================================================== */

	.hud-border {
		position: absolute;

		background:
			var(--hud-pink);

		opacity: 0.68;

		animation:
			hud-glitch
			5s
			steps(3, end)
			infinite;
	}

	.hud-top {
		left: 6%;
		right: 7%;

		top: 13px;

		height: 1px;
	}

	.hud-bottom {
		left: 6%;
		right: 6%;

		bottom: 14px;

		height: 1px;
	}

	.hud-left {
		left: 7px;

		top: 8%;
		bottom: 18%;

		width: 1px;
	}

	.hud-right {
		right: 7px;

		top: 8%;
		bottom: 18%;

		width: 1px;
	}

	/* =====================================================
	   TOP SYSTEM
	   ===================================================== */

	.top-system {
		position: absolute;

		left: 37%;
		top: 0;

		width: 27%;
		height: 35px;
	}

	.top-line {
		position: absolute;

		left: 0;
		right: 0;

		top: 13px;

		height: 1px;

		background:
			var(--hud-pink-soft);
	}

	.top-node {
		position: absolute;

		top: 9px;

		width: 7px;
		height: 7px;

		border:
			1px solid
			var(--hud-pink);

		background: #000;

		transform:
			rotate(45deg);
	}

	.node-a {
		left: 18%;
	}

	.node-b {
		left: 50%;
	}

	.node-c {
		right: 12%;
	}

	.top-bars {
		position: absolute;

		top: 24px;
		left: 37%;

		display: flex;

		gap: 3px;
	}

	.top-bars span {
		width: 6px;
		height: 4px;

		background:
			var(--hud-pink-soft);

		animation:
			bar-pulse
			1.6s
			steps(4, end)
			infinite;
	}

	/* =====================================================
	   SYSTEM MODULE
	   ===================================================== */

	.system-module {
		position: absolute;

		right: 4%;
		top: 24px;

		width: 120px;
		height: 32px;

		border:
			1px solid
			var(--hud-pink-soft);

		font-family:
			'Courier New',
			Courier,
			monospace;

		animation:
			module-glitch
			4.7s
			steps(3, end)
			infinite;
	}

	.system-label {
		position: absolute;

		left: 6px;
		top: 6px;

		font-size: 6px;
	}

	.system-bars {
		position: absolute;

		left: 28px;
		top: 7px;

		display: flex;

		gap: 2px;
	}

	.system-bars span {
		width: 6px;
		height: 5px;

		background:
			var(--hud-pink-soft);

		animation:
			bar-pulse
			1.5s
			steps(4, end)
			infinite;
	}

	.system-status {
		position: absolute;

		left: 28px;
		bottom: 4px;

		font-size: 4px;

		color:
			var(--hud-pink-soft);
	}

	/* =====================================================
	   RADARS
	   ===================================================== */

	.radar {
		position: absolute;

		top: 40%;

		width: 95px;
		height: 95px;

		opacity: 0.28;

		animation:
			module-glitch
			5.3s
			steps(3, end)
			infinite;
	}

	.radar-left {
		left: 25px;
	}

	.radar-right {
		right: 25px;
	}

	.radar-ring {
		position: absolute;

		left: 50%;
		top: 50%;

		border:
			1px solid
			var(--hud-pink-soft);

		border-radius: 50%;

		transform:
			translate(-50%, -50%);
	}

	.radar-ring-a {
		width: 90px;
		height: 90px;
	}

	.radar-ring-b {
		width: 58px;
		height: 58px;
	}

	.radar-ring-c {
		width: 26px;
		height: 26px;
	}

	.radar-horizontal,
	.radar-vertical {
		position: absolute;

		background:
			var(--hud-pink-soft);
	}

	.radar-horizontal {
		left: 0;
		right: 0;

		top: 50%;

		height: 1px;
	}

	.radar-vertical {
		top: 0;
		bottom: 0;

		left: 50%;

		width: 1px;
	}

	.radar-sweep {
		position: absolute;

		left: 50%;
		top: 50%;

		width: 40px;
		height: 1px;

		transform-origin:
			left center;

		background:
			linear-gradient(
				90deg,
				var(--hud-pink),
				transparent
			);

		animation:
			radar-spin
			3.4s
			linear
			infinite;
	}

	.radar-dot {
		position: absolute;

		left: 69%;
		top: 29%;

		width: 4px;
		height: 4px;

		border-radius: 50%;

		background:
			var(--hud-pink);
	}

	/* =====================================================
	   DATA RAILS
	   ===================================================== */

	.data-rail {
		position: absolute;

		top: 26%;

		width: 25px;

		font-family:
			'Courier New',
			Courier,
			monospace;

		opacity: 0.38;

		animation:
			module-glitch
			5.6s
			steps(3, end)
			infinite;
	}

	.data-left {
		left: 2px;
	}

	.data-right {
		right: 2px;
	}

	.rail-number {
		font-size: 5px;

		margin-bottom: 8px;
	}

	.rail-line {
		width: 1px;
		height: 195px;

		margin-left: 5px;

		background:
			var(--hud-pink-soft);
	}

	.rail-dot {
		display: block;

		width: 6px;
		height: 6px;

		margin:
			12px 0
			0 2px;

		border:
			1px solid
			var(--hud-pink);

		border-radius: 50%;

		background: #000;

		animation:
			node-pulse
			2.5s
			ease-in-out
			infinite;
	}

	.rail-code {
		display: flex;

		flex-direction: column;

		gap: 2px;

		margin-top: 9px;

		font-size: 4px;
	}

	/* =====================================================
	   BOTTOM MODULES
	   ===================================================== */

	.bottom-module {
		position: absolute;

		bottom: 36px;

		width: 180px;

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size: 6px;

		opacity: 0.5;

		animation:
			module-glitch
			5.1s
			steps(3, end)
			infinite;
	}

	.bottom-left-module {
		left: 3.5%;
	}

	.bottom-right-module {
		right: 3.5%;
	}

	.bottom-title {
		letter-spacing:
			0.14em;
	}

	.bottom-bars {
		display: flex;

		gap: 4px;

		margin-top: 7px;
	}

	.bottom-bars span {
		width: 17px;
		height: 4px;

		background:
			var(--hud-pink-soft);

		animation:
			bar-pulse
			1.7s
			steps(4, end)
			infinite;
	}

	.bottom-bars.compact span {
		width: 14px;
	}

	.bottom-rule {
		width: 100%;
		height: 1px;

		margin-top: 7px;

		background:
			var(--hud-pink-soft);
	}

	.bottom-small {
		margin-top: 5px;

		color:
			var(--hud-pink-soft);

		font-size: 5px;
	}

	/* =====================================================
	   BOTTOM STREAM
	   ===================================================== */

	.bottom-stream {
		position: absolute;

		left: 50%;
		bottom: 20px;

		width: 390px;
		height: 31px;

		transform:
			translateX(-50%);

		opacity: 0.45;

		font-family:
			'Courier New',
			Courier,
			monospace;

		animation:
			stream-glitch
			5s
			steps(3, end)
			infinite;
	}

	.stream-line {
		position: absolute;

		left: 0;
		right: 0;

		top: 4px;

		height: 1px;

		background:
			var(--hud-pink-soft);
	}

	.stream-node {
		position: absolute;

		top: 1px;

		width: 7px;
		height: 7px;

		border:
			1px solid
			var(--hud-pink);

		border-radius: 50%;

		background: #000;
	}

	.stream-start {
		left: 0;
	}

	.stream-middle {
		left: 50%;

		transform:
			translateX(-50%);
	}

	.stream-end {
		right: 0;
	}

	.stream-bars {
		position: absolute;

		left: 55px;
		right: 55px;

		top: 13px;

		display: flex;

		gap: 4px;
	}

	.stream-bars span {
		flex: 1;

		height: 3px;

		background:
			var(--hud-pink-soft);

		animation:
			bar-pulse
			1.7s
			steps(4, end)
			infinite;
	}

	.stream-text {
		position: absolute;

		left: 50%;
		bottom: -1px;

		transform:
			translateX(-50%);

		font-size: 4px;

		letter-spacing:
			0.15em;
	}

	/* =====================================================
	   CORNERS
	   ===================================================== */

	.corner {
		position: absolute;

		width: 80px;
		height: 52px;

		animation:
			module-glitch
			5.4s
			steps(3, end)
			infinite;
	}

	.corner-tl {
		left: 10px;
		top: 16px;
	}

	.corner-tr {
		right: 10px;
		top: 16px;

		transform:
			scaleX(-1);
	}

	.corner-bl {
		left: 10px;
		bottom: 16px;

		transform:
			scaleY(-1);
	}

	.corner-br {
		right: 10px;
		bottom: 16px;

		transform:
			scale(-1);
	}

	.corner-long,
	.corner-short {
		position: absolute;

		left: 0;

		height: 1px;

		background:
			var(--hud-pink);
	}

	.corner-long {
		top: 13px;

		width: 60px;
	}

	.corner-long::after {
		content: '';

		position: absolute;

		right: 0;
		top: 0;

		width: 1px;
		height: 18px;

		background:
			var(--hud-pink);
	}

	.corner-short {
		top: 29px;

		width: 38px;

		opacity: 0.55;
	}

	.corner-dot {
		position: absolute;

		left: 57px;
		top: 10px;

		width: 7px;
		height: 7px;

		border:
			1px solid
			var(--hud-pink);

		border-radius: 50%;

		background: #000;
	}

	/* =====================================================
	   GLOBAL HUD ANIMATIONS
	   ===================================================== */

	@keyframes hud-glitch {
		0%,
		86%,
		100% {
			opacity: 0.68;

			transform:
				translate(0, 0);
		}

		88% {
			opacity: 0.35;

			transform:
				translate(-1px, 0);
		}

		90% {
			opacity: 0.95;

			transform:
				translate(2px, 0);
		}

		92% {
			opacity: 0.45;

			transform:
				translate(-1px, 1px);
		}

		94% {
			opacity: 0.68;

			transform:
				translate(0, 0);
		}
	}

	@keyframes module-glitch {
		0%,
		84%,
		100% {
			opacity: 0.5;

			filter: none;
		}

		86% {
			opacity: 0.25;

			filter:
				drop-shadow(
					-2px 0
					rgba(
						255,
						0,
						128,
						0.4
					)
				);
		}

		88% {
			opacity: 0.78;

			filter:
				drop-shadow(
					2px 0
					rgba(
						255,
						255,
						255,
						0.18
					)
				);
		}

		91% {
			opacity: 0.35;
		}

		94% {
			opacity: 0.5;

			filter: none;
		}
	}

	@keyframes bar-pulse {
		0%,
		68%,
		100% {
			opacity: 0.5;

			transform:
				scaleX(1);
		}

		72% {
			opacity: 0.16;

			transform:
				scaleX(0.6);
		}

		77% {
			opacity: 0.9;

			transform:
				scaleX(1.15);
		}

		82% {
			opacity: 0.3;

			transform:
				scaleX(0.82);
		}

		87% {
			opacity: 0.6;

			transform:
				scaleX(1);
		}
	}

	@keyframes node-pulse {
		0%,
		100% {
			opacity: 0.35;

			transform:
				scale(0.9);
		}

		50% {
			opacity: 0.95;

			transform:
				scale(1.15);
		}
	}

	@keyframes radar-spin {
		from {
			transform:
				rotate(0deg);
		}

		to {
			transform:
				rotate(360deg);
		}
	}

	@keyframes stream-glitch {
		0%,
		87%,
		100% {
			opacity: 0.45;

			transform:
				translateX(-50%);
		}

		89% {
			opacity: 0.22;

			transform:
				translateX(
					calc(
						-50% - 2px
					)
				);
		}

		91% {
			opacity: 0.7;

			transform:
				translateX(
					calc(
						-50% + 2px
					)
				);
		}

		94% {
			opacity: 0.45;

			transform:
				translateX(-50%);
		}
	}

	/* =====================================================
	   BOOT SCREEN
	   ===================================================== */

	.boot-screen {
		position: fixed;

		inset: 0;

		z-index: 10000;

		overflow: hidden;

		background: #000;

		pointer-events: none;
	}

	.boot-grid {
		position: absolute;

		inset: 0;

		background:
			linear-gradient(
				rgba(
					255,
					0,
					128,
					0.025
				)
				1px,
				transparent 1px
			),
			linear-gradient(
				90deg,
				rgba(
					255,
					0,
					128,
					0.025
				)
				1px,
				transparent 1px
			);

		background-size:
			34px 34px;

		opacity: 0.35;
	}

	.boot-vignette {
		position: absolute;

		inset: 0;

		background:
			radial-gradient(
				ellipse at center,
				rgba(
					255,
					0,
					128,
					0.035
				),
				#000 78%
			);
	}

	.boot-glitches {
		position: absolute;

		inset: 0;
	}

	.boot-glitch {
		position: absolute;

		height: 2px;

		opacity: 0;

		background:
			linear-gradient(
				90deg,
				transparent,
				var(--hud-pink),
				transparent
			);

		animation:
			boot-glitch
			1.25s
			steps(7, end)
			infinite;
	}

	.bg1 {
		top: 17%;
		left: 0;
		width: 52%;
	}

	.bg2 {
		top: 27%;
		right: 4%;
		width: 42%;

		animation-delay:
			0.07s;
	}

	.bg3 {
		top: 38%;
		left: 12%;
		width: 69%;

		animation-delay:
			0.14s;
	}

	.bg4 {
		top: 48%;
		right: 0;
		width: 55%;

		animation-delay:
			0.21s;
	}

	.bg5 {
		top: 58%;
		left: 3%;
		width: 63%;

		animation-delay:
			0.28s;
	}

	.bg6 {
		top: 68%;
		right: 9%;
		width: 62%;

		animation-delay:
			0.35s;
	}

	.bg7 {
		top: 79%;
		left: 16%;
		width: 53%;

		animation-delay:
			0.42s;
	}

	.bg8 {
		top: 88%;
		right: 2%;
		width: 47%;

		animation-delay:
			0.49s;
	}

	.boot-interface {
		position: absolute;

		inset: 0;

		display: grid;

		place-items: center;
	}

	.boot-frame {
		position: relative;

		width:
			min(
				480px,
				62vw
			);

		height:
			min(
				240px,
				35vh
			);

		border:
			1px solid
			var(--hud-pink-soft);

		background:
			rgba(
				0,
				0,
				0,
				0.72
			);

		animation:
			boot-frame-in
			700ms
			cubic-bezier(
				0.22,
				1,
				0.36,
				1
			)
			forwards;
	}

	.boot-corner {
		position: absolute;

		width: 24px;
		height: 24px;

		border-color:
			var(--hud-pink);
	}

	.btl {
		left: -1px;
		top: -1px;

		border-left: 1px solid;
		border-top: 1px solid;
	}

	.btr {
		right: -1px;
		top: -1px;

		border-right: 1px solid;
		border-top: 1px solid;
	}

	.bbl {
		left: -1px;
		bottom: -1px;

		border-left: 1px solid;
		border-bottom: 1px solid;
	}

	.bbr {
		right: -1px;
		bottom: -1px;

		border-right: 1px solid;
		border-bottom: 1px solid;
	}

	.boot-circle {
		position: absolute;

		left: 50%;
		top: 48%;

		border:
			1px solid
			var(--hud-pink-soft);

		border-radius: 50%;

		transform:
			translate(-50%, -50%);

		animation:
			boot-ring
			2s
			ease-out
			infinite;
	}

	.boot-circle-large {
		width: 110px;
		height: 110px;
	}

	.boot-circle-small {
		width: 50px;
		height: 50px;

		animation-delay:
			0.22s;
	}

	.boot-cross {
		position: absolute;

		left: 50%;
		top: 48%;

		background:
			var(--hud-pink-soft);

		transform:
			translate(-50%, -50%);
	}

	.boot-cross-h {
		width: 145px;
		height: 1px;
	}

	.boot-cross-v {
		width: 1px;
		height: 145px;
	}

	.boot-title {
		position: absolute;

		left: 50%;
		top: 39%;

		transform:
			translateX(-50%);

		color:
			var(--hud-pink);

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size:
			clamp(
				0.72rem,
				1.1vw,
				1rem
			);

		letter-spacing:
			0.13em;

		white-space: nowrap;
	}

	.boot-subtitle {
		position: absolute;

		left: 50%;
		top: 55%;

		transform:
			translateX(-50%);

		color:
			rgba(
				255,
				0,
				128,
				0.65
			);

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size: 5px;

		letter-spacing:
			0.15em;

		white-space: nowrap;
	}

	.boot-progress {
		position: absolute;

		left: 50%;
		bottom: 18px;

		transform:
			translateX(-50%);

		display: flex;

		gap: 4px;
	}

	.boot-progress span {
		width: 8px;
		height: 3px;

		background:
			var(--hud-pink-soft);

		animation:
			bar-pulse
			1.3s
			steps(4, end)
			infinite;
	}

	.boot-flash {
		position: absolute;

		inset: 0;

		background:
			var(--hud-pink);

		opacity: 0;
	}

	.boot-exiting {
		animation:
			boot-exit
			1s
			cubic-bezier(
				0.65,
				0,
				0.35,
				1
			)
			forwards;
	}

	.boot-exiting
		.boot-flash {
		animation:
			boot-flash
			1s
			ease
			forwards;
	}

	/* =====================================================
	   ROUTE TRANSITION
	   ===================================================== */

	.route-transition {
		position: fixed;

		inset: 0;

		z-index: 9999;

		overflow: hidden;

		pointer-events: none;
	}

	.transition-black {
		position: absolute;

		inset: 0;

		background:
			rgba(
				0,
				0,
				0,
				0.96
			);

		animation:
			transition-black-in
			430ms
			ease-out
			forwards;
	}

	.transition-scanlines {
		position: absolute;

		inset: 0;

		background:
			repeating-linear-gradient(
				180deg,
				transparent 0,
				transparent 5px,
				rgba(
					255,
					0,
					128,
					0.045
				)
				6px
			);

		opacity: 0.55;
	}

	.transition-glitches {
		position: absolute;

		inset: 0;
	}

	.transition-strip {
		position: absolute;

		height: 3px;

		opacity: 0;

		background:
			linear-gradient(
				90deg,
				transparent,
				var(--hud-pink-soft),
				var(--hud-pink),
				var(--hud-pink-soft),
				transparent
			);

		box-shadow:
			0 0 14px
			rgba(
				255,
				0,
				128,
				0.3
			);

		animation:
			route-glitch
			900ms
			cubic-bezier(
				0.65,
				0,
				0.35,
				1
			)
			forwards;
	}

	.t1 {
		top: 12%;
		left: 0;
		width: 55%;
	}

	.t2 {
		top: 20%;
		left: 34%;
		width: 64%;

		animation-delay:
			0.03s;
	}

	.t3 {
		top: 28%;
		left: -5%;
		width: 48%;

		animation-delay:
			0.06s;
	}

	.t4 {
		top: 36%;
		left: 13%;
		width: 73%;

		animation-delay:
			0.09s;
	}

	.t5 {
		top: 44%;
		left: 43%;
		width: 57%;

		animation-delay:
			0.12s;
	}

	.t6 {
		top: 52%;
		left: -2%;
		width: 57%;

		animation-delay:
			0.15s;
	}

	.t7 {
		top: 60%;
		left: 18%;
		width: 69%;

		animation-delay:
			0.18s;
	}

	.t8 {
		top: 68%;
		left: 45%;
		width: 58%;

		animation-delay:
			0.21s;
	}

	.t9 {
		top: 75%;
		left: 3%;
		width: 55%;

		animation-delay:
			0.24s;
	}

	.t10 {
		top: 82%;
		left: 27%;
		width: 69%;

		animation-delay:
			0.27s;
	}

	.t11 {
		top: 88%;
		left: 8%;
		width: 43%;

		animation-delay:
			0.3s;
	}

	.t12 {
		top: 93%;
		left: 51%;
		width: 45%;

		animation-delay:
			0.33s;
	}

	.transition-interface {
		position: absolute;

		inset: 0;

		display: grid;

		place-items: center;

		opacity: 0;

		animation:
			transition-interface-in
			650ms
			180ms
			ease-out
			forwards;
	}

	.transition-circle {
		position: absolute;

		left: 50%;
		top: 50%;

		border:
			1px solid
			var(--hud-pink-soft);

		border-radius: 50%;

		transform:
			translate(-50%, -50%);

		animation:
			boot-ring
			1.7s
			ease-out
			infinite;
	}

	.transition-circle.outer {
		width: 145px;
		height: 145px;
	}

	.transition-circle.inner {
		width: 52px;
		height: 52px;

		animation-delay:
			0.22s;
	}

	.transition-title {
		position: absolute;

		left: 50%;
		top: 44%;

		transform:
			translateX(-50%);

		color:
			rgba(
				255,
				255,
				255,
				0.68
			);

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size:
			clamp(
				0.65rem,
				0.9vw,
				0.85rem
			);

		letter-spacing:
			0.15em;

		white-space: nowrap;
	}

	.transition-code {
		position: absolute;

		left: 50%;
		top: 55%;

		transform:
			translateX(-50%);

		color:
			rgba(
				255,
				255,
				255,
				0.35
			);

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size: 6px;

		letter-spacing:
			0.12em;
	}

	.transition-code span {
		color:
			var(--hud-pink);
	}

	.transition-progress {
		position: absolute;

		left: 50%;
		top: 61%;

		transform:
			translateX(-50%);

		display: flex;

		gap: 4px;
	}

	.transition-progress span {
		width: 7px;
		height: 3px;

		background:
			var(--hud-pink-soft);

		animation:
			bar-pulse
			1.2s
			steps(4, end)
			infinite;
	}

	.transition-flash {
		position: absolute;

		inset: 0;

		background:
			var(--hud-pink);

		opacity: 0;
	}

	.transition-leaving
		.transition-black {
		animation:
			transition-black-out
			720ms
			ease
			forwards;
	}

	.transition-leaving
		.transition-interface {
		animation:
			transition-interface-out
			650ms
			ease
			forwards;
	}

	.transition-leaving
		.transition-flash {
		animation:
			transition-flash
			720ms
			ease
			forwards;
	}

	/* =====================================================
	   BOOT / TRANSITION KEYFRAMES
	   ===================================================== */

	@keyframes boot-glitch {
		0% {
			opacity: 0;

			transform:
				translateX(-14%)
				scaleX(0.7);
		}

		18% {
			opacity: 0.35;
		}

		32% {
			opacity: 0.8;

			transform:
				translateX(4%)
				scaleX(1.06);
		}

		48% {
			opacity: 0.42;

			transform:
				translateX(-2%)
				scaleX(0.92);
		}

		64% {
			opacity: 0.85;

			transform:
				translateX(5%)
				scaleX(1.04);
		}

		82% {
			opacity: 0.22;

			transform:
				translateX(10%)
				scaleX(0.78);
		}

		100% {
			opacity: 0;

			transform:
				translateX(16%)
				scaleX(0.65);
		}
	}

	@keyframes boot-frame-in {
		from {
			opacity: 0;

			transform:
				scale(1.08);
		}

		to {
			opacity: 1;

			transform:
				scale(1);
		}
	}

	@keyframes boot-ring {
		0% {
			opacity: 0.12;

			transform:
				translate(-50%, -50%)
				scale(0.8);
		}

		45% {
			opacity: 0.5;

			transform:
				translate(-50%, -50%)
				scale(1);
		}

		100% {
			opacity: 0;

			transform:
				translate(-50%, -50%)
				scale(1.25);
		}
	}

	@keyframes boot-exit {
		0% {
			opacity: 1;

			transform:
				scale(1);
		}

		35% {
			opacity: 0.95;

			transform:
				scale(1.006)
				translateX(-2px);
		}

		60% {
			opacity: 0.72;

			transform:
				scale(1.015)
				translateX(3px);
		}

		80% {
			opacity: 0.35;

			transform:
				scale(1.028)
				translateX(-4px);
		}

		100% {
			opacity: 0;

			transform:
				scale(1.04)
				translateX(6px);
		}
	}

	@keyframes boot-flash {
		0%,
		55% {
			opacity: 0;
		}

		66% {
			opacity: 0.025;
		}

		72% {
			opacity: 0;
		}

		81% {
			opacity: 0.055;
		}

		87% {
			opacity: 0;
		}

		100% {
			opacity: 0;
		}
	}

	@keyframes transition-black-in {
		from {
			opacity: 0;
		}

		to {
			opacity: 1;
		}
	}

	@keyframes transition-black-out {
		0% {
			opacity: 1;
		}

		20% {
			opacity: 0.96;
		}

		40% {
			opacity: 0.82;
		}

		60% {
			opacity: 0.55;
		}

		80% {
			opacity: 0.24;
		}

		100% {
			opacity: 0;
		}
	}

	@keyframes route-glitch {
		0% {
			opacity: 0;

			transform:
				translateX(-12%)
				scaleX(0.66);
		}

		12% {
			opacity: 0.12;
		}

		26% {
			opacity: 0.52;

			transform:
				translateX(-1%)
				scaleX(0.92);
		}

		40% {
			opacity: 0.95;

			transform:
				translateX(5%)
				scaleX(1.08);
		}

		55% {
			opacity: 0.68;

			transform:
				translateX(-2%)
				scaleX(0.95);
		}

		70% {
			opacity: 0.43;

			transform:
				translateX(5%)
				scaleX(0.86);
		}

		85% {
			opacity: 0.16;

			transform:
				translateX(11%)
				scaleX(0.72);
		}

		100% {
			opacity: 0;

			transform:
				translateX(18%)
				scaleX(0.62);
		}
	}

	@keyframes transition-interface-in {
		from {
			opacity: 0;

			transform:
				scale(1.07);
		}

		to {
			opacity: 1;

			transform:
				scale(1);
		}
	}

	@keyframes transition-interface-out {
		0% {
			opacity: 1;

			transform:
				scale(1);
		}

		35% {
			opacity: 0.9;

			transform:
				scale(1.01);
		}

		65% {
			opacity: 0.55;

			transform:
				scale(1.04);
		}

		100% {
			opacity: 0;

			transform:
				scale(1.08);
		}
	}

	@keyframes transition-flash {
		0%,
		38% {
			opacity: 0;
		}

		48% {
			opacity: 0.035;
		}

		55% {
			opacity: 0;
		}

		67% {
			opacity: 0.02;
		}

		74% {
			opacity: 0;
		}

		100% {
			opacity: 0;
		}
	}

	/* =====================================================
	   RESPONSIVE
	   ===================================================== */

	@media (max-width: 900px) {
		.home-button {
			left: 3%;
			top: 2%;

			width: 190px;
			height: 78px;
		}

		.radar,
		.data-rail {
			opacity: 0.15;
		}

		.bottom-module {
			opacity: 0.25;
		}

		.bottom-stream {
			width: 300px;
		}
	}

	@media (max-width: 650px) {
		.home-button {
			left: 4%;
			top: 2%;

			width: 150px;
			height: 65px;
		}

		.radar,
		.data-rail,
		.bottom-module {
			opacity: 0.08;
		}

		.bottom-stream {
			width: 230px;
		}

		.system-module {
			opacity: 0.4;
		}

		.boot-frame {
			width: 76vw;
			height: 30vh;
		}
	}

	/* =====================================================
	   REDUCED MOTION
	   ===================================================== */

	@media (prefers-reduced-motion: reduce) {
		.global-hud * {
			animation: none !important;
		}

		.signature-glitch,
		.signature-scan {
			display: none;
		}

		.home-button:hover,
		.home-button:focus-visible {
			transform: none;
		}

		.boot-screen,
		.route-transition {
			animation: none;
		}
	}
</style>
