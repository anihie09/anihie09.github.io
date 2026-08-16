<script lang="ts">
	import InteractiveAscii from '$lib/InteractiveAscii.svelte';
</script>

<svelte:head>
	<title>ANI — Mechanical Engineering</title>

	<meta
		name="description"
		content="ANI — Mechanical Engineering portfolio"
	/>
</svelte:head>

<div class="home-page">

	<!-- =================================================
	     INTERACTIVE HOMEPAGE BACKGROUND
	     ================================================= -->

	<div class="home-background">
		<InteractiveAscii
			src="/home-background.png"
			alt="Interactive homepage background"
			characters=" .:-=+*#%@"
			color="#ff0080"
			background="#000000"
			cellSize={6}
			cursorRadius={155}
			zoom={1.12}
			noiseInterval={100}
			opacity={0.9}
		/>
	</div>


	<!-- =================================================
	     DARK FADE

	     Keeps the background atmospheric while allowing
	     the navigation to remain readable.
	     ================================================= -->

	<div
		class="background-fade"
		aria-hidden="true"
	></div>


	<!-- =================================================
	     LEFT SYSTEM INFORMATION
	     ================================================= -->

	<section
		class="system-info"
		aria-label="Profile information"
	>

		<div class="system-title">
			SYSTEMS UPDATING
			<span class="blink-dots">...</span>
		</div>

		<div class="system-lines">
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

	</section>


	<!-- =================================================
	     MAIN PERSON / PROFILE AREA

	     This frame gives the homepage the same visual
	     relationship it had before the InteractiveAscii
	     change.
	     ================================================= -->

	<section class="profile-frame">

		<div class="profile-label">
			// ANI_LE v1
		</div>

		<div class="profile-top"></div>
		<div class="profile-left"></div>
		<div class="profile-right"></div>
		<div class="profile-bottom"></div>

		<div
			class="profile-target"
			aria-hidden="true"
		>
			<div></div>
		</div>

	</section>


	<!-- =================================================
	     RIGHT NAVIGATION

	     RESTORED FROM THE PREVIOUS HOMEPAGE DESIGN.
	     ================================================= -->

	<nav
		class="navigation-panel"
		aria-label="Main navigation"
	>

		<div class="directory">

			<div class="directory-title">
				DIRECTORY
			</div>

			<div class="directory-line"></div>

			<div class="directory-status">

				<span>
					SYS
				</span>

				<strong>
					ACTIVE
				</strong>

			</div>

			<div class="directory-status">

				<span>
					MODE
				</span>

				<strong>
					01
				</strong>

			</div>

			<div class="directory-status">

				<span>
					LINK
				</span>

				<strong>
					ONLINE
				</strong>

			</div>

		</div>


		<div class="navigation-list">

			<a
				class="navigation-link"
				href="/about"
			>
				<span class="navigation-number">
					01
				</span>

				<span class="navigation-name">
					ABOUT
				</span>

				<span
					class="navigation-arrow"
					aria-hidden="true"
				>
					&gt;
				</span>
			</a>


			<a
				class="navigation-link"
				href="/projects"
			>
				<span class="navigation-number">
					02
				</span>

				<span class="navigation-name">
					PROJECTS
				</span>

				<span
					class="navigation-arrow"
					aria-hidden="true"
				>
					&gt;
				</span>
			</a>


			<a
				class="navigation-link"
				href="/contact"
			>
				<span class="navigation-number">
					03
				</span>

				<span class="navigation-name">
					CONTACT
				</span>

				<span
					class="navigation-arrow"
					aria-hidden="true"
				>
					&gt;
				</span>
			</a>

		</div>

	</nav>


	<!-- =================================================
	     DECORATIVE SIDE TARGETS
	     ================================================= -->

	<div
		class="side-target target-left"
		aria-hidden="true"
	>
		<div class="target-ring ring-1"></div>
		<div class="target-ring ring-2"></div>
		<div class="target-ring ring-3"></div>
		<div class="target-cross horizontal"></div>
		<div class="target-cross vertical"></div>
		<div class="target-dot"></div>
	</div>


	<div
		class="side-target target-right"
		aria-hidden="true"
	>
		<div class="target-ring ring-1"></div>
		<div class="target-ring ring-2"></div>
		<div class="target-ring ring-3"></div>
		<div class="target-cross horizontal"></div>
		<div class="target-cross vertical"></div>
		<div class="target-dot"></div>
	</div>


	<!-- =================================================
	     BOTTOM STATUS
	     ================================================= -->

	<div class="bottom-status left-status">

		<div class="status-title">
			CORE / 02
		</div>

		<div class="status-bars">
			<span></span>
			<span></span>
			<span></span>
			<span></span>
			<span></span>
			<span></span>
		</div>

		<div class="status-text">
			SYSTEM READY
		</div>

	</div>


	<div class="bottom-status right-status">

		<div class="status-title">
			STATUS
		</div>

		<div class="status-text">
			01-09 / ACTIVE
		</div>

		<div class="status-bars">
			<span></span>
			<span></span>
			<span></span>
			<span></span>
			<span></span>
		</div>

	</div>


	<!-- =================================================
	     BOTTOM CENTER LINK
	     ================================================= -->

	<div class="bottom-link">

		<div class="bottom-link-line"></div>

		<div class="bottom-link-node node-left"></div>
		<div class="bottom-link-node node-center"></div>
		<div class="bottom-link-node node-right"></div>

		<div class="bottom-link-text">
			LINK ESTABLISHED
		</div>

	</div>

</div>


<style>
	/* =====================================================
	   PAGE
	   ===================================================== */

	.home-page {
		position: relative;

		width: 100vw;
		height: 100vh;

		min-height: 100vh;

		overflow: hidden;

		background:
			#000;

		color:
			#fff;

		font-family:
			'Courier New',
			Courier,
			monospace;
	}


	/* =====================================================
	   BACKGROUND
	   ===================================================== */

	.home-background {
		position: absolute;

		inset: 0;

		z-index: 1;

		overflow: hidden;
	}

	.background-fade {
		position: absolute;

		inset: 0;

		z-index: 2;

		pointer-events: none;

		background:
			linear-gradient(
				90deg,
				rgba(
					0,
					0,
					0,
					0.2
				),
				transparent 36%,
				rgba(
					0,
					0,
					0,
					0.24
				)
			),
			linear-gradient(
				180deg,
				rgba(
					0,
					0,
					0,
					0.2
				),
				transparent 25%,
				rgba(
					0,
					0,
					0,
					0.22
				)
			);
	}


	/* =====================================================
	   SYSTEM INFO
	   ===================================================== */

	.system-info {
		position: absolute;

		left: 2.7%;
		top: 5.5%;

		z-index: 20;

		color:
			rgba(
				255,
				255,
				255,
				0.86
			);

		font-size:
			clamp(
				8px,
				0.72vw,
				12px
			);

		line-height:
			1.7;

		letter-spacing:
			0.03em;

		text-shadow:
			0 0 8px
			rgba(
				255,
				0,
				128,
				0.08
			);
	}

	.system-title {
		color:
			#fff;

		margin-bottom:
			4px;

		letter-spacing:
			0.05em;
	}

	.blink-dots {
		display:
			inline-block;

		width: 22px;

		color:
			#fff;

		overflow: hidden;

		animation:
			system-dots
			1.2s
			steps(4, end)
			infinite;
	}


	/* =====================================================
	   PROFILE FRAME
	   ===================================================== */

	.profile-frame {
		position: absolute;

		left: 48.5%;
		top: 17%;

		width: 28%;
		height: 49%;

		z-index: 12;

		pointer-events:
			none;

		border:
			1px solid
			rgba(
				255,
				0,
				128,
				0.7
			);

		transform:
			translateX(-50%);

		opacity:
			0.95;
	}

	.profile-label {
		position: absolute;

		left: 0;
		top: -38px;

		padding:
			9px 17px;

		background:
			#ff0080;

		color:
			#000;

		font-size:
			clamp(
				8px,
				0.8vw,
				12px
			);

		font-weight:
			700;

		letter-spacing:
			0.05em;
	}

	.profile-top,
	.profile-bottom,
	.profile-left,
	.profile-right {
		position: absolute;

		background:
			#ff0080;

		opacity:
			0.7;
	}

	.profile-top {
		left: 0;
		right: 0;

		top: 0;

		height: 1px;
	}

	.profile-bottom {
		left: 0;
		right: 0;

		bottom: 0;

		height: 1px;
	}

	.profile-left {
		top: 0;
		bottom: 0;

		left: 0;

		width: 1px;
	}

	.profile-right {
		top: 0;
		bottom: 0;

		right: 0;

		width: 1px;
	}

	.profile-target {
		position: absolute;

		left: 50%;
		top: 51%;

		width: 52px;
		height: 52px;

		transform:
			translate(
				-50%,
				-50%
			);

		border:
			1px solid
			rgba(
				255,
				0,
				128,
				0.45
			);

		border-radius: 50%;
	}

	.profile-target::before,
	.profile-target::after {
		content: '';

		position: absolute;

		background:
			rgba(
				255,
				0,
				128,
				0.45
			);
	}

	.profile-target::before {
		left: -10px;
		right: -10px;

		top: 50%;

		height: 1px;
	}

	.profile-target::after {
		top: -10px;
		bottom: -10px;

		left: 50%;

		width: 1px;
	}

	.profile-target div {
		position: absolute;

		left: 50%;
		top: 50%;

		width: 16px;
		height: 16px;

		transform:
			translate(
				-50%,
				-50%
			);

		border:
			1px solid
			rgba(
				255,
				0,
				128,
				0.65
			);

		border-radius: 50%;
	}


	/* =====================================================
	   NAVIGATION PANEL
	   ===================================================== */

	.navigation-panel {
		position: absolute;

		right: 7.6%;
		top: 29%;

		z-index: 25;

		width:
			min(
				230px,
				18vw
			);

		color:
			#ff0080;

		font-family:
			'Courier New',
			Courier,
			monospace;
	}

	.directory {
		margin-bottom:
			28px;

		padding:
			0 10px 15px;

		border:
			1px solid
			rgba(
				255,
				0,
				128,
				0.42
			);

		background:
			rgba(
				0,
				0,
				0,
				0.24
			);
	}

	.directory-title {
		padding:
			11px 0 7px;

		font-size: 7px;

		letter-spacing:
			0.16em;
	}

	.directory-line {
		width: 100%;
		height: 1px;

		background:
			rgba(
				255,
				0,
				128,
				0.5
			);

		margin-bottom:
			10px;
	}

	.directory-status {
		display:
			flex;

		justify-content:
			space-between;

		margin-top:
			5px;

		font-size: 5px;

		letter-spacing:
			0.08em;

		color:
			rgba(
				255,
				0,
				128,
				0.68
			);
	}

	.navigation-list {
		display:
			flex;

		flex-direction:
			column;

		gap:
			22px;
	}

	.navigation-link {
		position: relative;

		display:
			grid;

		grid-template-columns:
			30px
			1fr
			15px;

		align-items:
			center;

		gap:
			8px;

		padding:
			5px 0;

		color:
			#ff0080;

		text-decoration:
			none;

		transition:
			transform
			180ms
			cubic-bezier(
				0.22,
				1,
				0.36,
				1
			),
			text-shadow
			180ms ease;
	}

	.navigation-number {
		font-size: 5px;

		color:
			rgba(
				255,
				0,
				128,
				0.5
			);
	}

	.navigation-name {
		font-size:
			clamp(
				10px,
				0.95vw,
				15px
			);

		font-weight:
			500;

		letter-spacing:
			0.05em;
	}

	.navigation-arrow {
		font-size: 10px;

		opacity: 0;

		transform:
			translateX(-5px);

		transition:
			opacity
			160ms ease,
			transform
			160ms ease;
	}

	.navigation-link::before {
		content: '';

		position: absolute;

		left: -12px;

		top: 50%;

		width: 2px;
		height: 0;

		background:
			#ff0080;

		transform:
			translateY(-50%);

		transition:
			height
			180ms ease;
	}

	.navigation-link:hover,
	.navigation-link:focus-visible {
		transform:
			translateX(7px);

		text-shadow:
			0 0 8px
			rgba(
				255,
				0,
				128,
				0.55
			);

		outline:
			none;
	}

	.navigation-link:hover::before,
	.navigation-link:focus-visible::before {
		height: 18px;
	}

	.navigation-link:hover
		.navigation-arrow,
	.navigation-link:focus-visible
		.navigation-arrow {
		opacity: 1;

		transform:
			translateX(0);
	}


	/* =====================================================
	   SIDE TARGETS
	   ===================================================== */

	.side-target {
		position: absolute;

		top: 47%;

		width: 90px;
		height: 90px;

		z-index: 10;

		opacity:
			0.45;
	}

	.target-left {
		left: 4%;
	}

	.target-right {
		right: 4%;
	}

	.target-ring {
		position: absolute;

		left: 50%;
		top: 50%;

		border:
			1px solid
			rgba(
				255,
				0,
				128,
				0.52
			);

		border-radius: 50%;

		transform:
			translate(
				-50%,
				-50%
			);
	}

	.ring-1 {
		width: 85px;
		height: 85px;
	}

	.ring-2 {
		width: 58px;
		height: 58px;
	}

	.ring-3 {
		width: 25px;
		height: 25px;
	}

	.target-cross {
		position: absolute;

		background:
			rgba(
				255,
				0,
				128,
				0.5
			);
	}

	.target-cross.horizontal {
		left: 0;
		right: 0;

		top: 50%;

		height: 1px;
	}

	.target-cross.vertical {
		top: 0;
		bottom: 0;

		left: 50%;

		width: 1px;
	}

	.target-dot {
		position: absolute;

		left: 69%;
		top: 30%;

		width: 4px;
		height: 4px;

		border-radius: 50%;

		background:
			#ff0080;
	}


	/* =====================================================
	   BOTTOM STATUS
	   ===================================================== */

	.bottom-status {
		position: absolute;

		bottom: 31px;

		z-index: 20;

		font-size: 5px;

		color:
			rgba(
				255,
				0,
				128,
				0.58
			);
	}

	.left-status {
		left: 3.5%;
	}

	.right-status {
		right: 4%;
	}

	.status-title {
		margin-bottom:
			7px;

		letter-spacing:
			0.14em;
	}

	.status-text {
		margin-top:
			7px;
	}

	.status-bars {
		display:
			flex;

		gap: 4px;
	}

	.status-bars span {
		width: 18px;
		height: 4px;

		background:
			rgba(
				255,
				0,
				128,
				0.5
			);

		animation:
			status-bars
			1.7s
			steps(4, end)
			infinite;
	}


	/* =====================================================
	   BOTTOM CENTER
	   ===================================================== */

	.bottom-link {
		position: absolute;

		left: 50%;
		bottom: 21px;

		width: 380px;
		height: 30px;

		z-index: 20;

		transform:
			translateX(-50%);

		color:
			rgba(
				255,
				0,
				128,
				0.55
			);
	}

	.bottom-link-line {
		position: absolute;

		left: 0;
		right: 0;

		top: 3px;

		height: 1px;

		background:
			rgba(
				255,
				0,
				128,
				0.42
			);
	}

	.bottom-link-node {
		position: absolute;

		top: 0;

		width: 7px;
		height: 7px;

		border:
			1px solid
			#ff0080;

		border-radius: 50%;

		background:
			#000;
	}

	.bottom-link-node.node-left {
		left: 0;
	}

	.bottom-link-node.node-center {
		left: 50%;

		transform:
			translateX(-50%);
	}

	.bottom-link-node.node-right {
		right: 0;
	}

	.bottom-link-text {
		position: absolute;

		left: 50%;
		bottom: 0;

		transform:
			translateX(-50%);

		font-size: 4px;

		letter-spacing:
			0.18em;
	}


	/* =====================================================
	   ANIMATIONS
	   ===================================================== */

	@keyframes system-dots {
		0% {
			width: 3px;
		}

		25% {
			width: 7px;
		}

		50% {
			width: 12px;
		}

		75% {
			width: 17px;
		}

		100% {
			width: 22px;
		}
	}

	@keyframes status-bars {
		0%,
		70%,
		100% {
			opacity:
				0.45;

			transform:
				scaleX(1);
		}

		76% {
			opacity:
				0.1;

			transform:
				scaleX(0.65);
		}

		83% {
			opacity:
				0.95;

			transform:
				scaleX(1.08);
		}
	}


	/* =====================================================
	   RESPONSIVE
	   ===================================================== */

	@media (max-width: 1000px) {
		.navigation-panel {
			right: 5%;
		}

		.profile-frame {
			left: 47%;
			width: 32%;
		}
	}

	@media (max-width: 760px) {
		.navigation-panel {
			right: 5%;
			top: auto;
			bottom: 13%;
			width: 190px;
		}

		.profile-frame {
			left: 50%;
			top: 18%;
			width: 40%;
			height: 40%;
		}

		.side-target {
			opacity:
				0.2;
		}

		.bottom-link {
			width: 280px;
		}
	}

	@media (max-width: 520px) {
		.system-info {
			left: 4%;
			top: 8%;
		}

		.navigation-panel {
			right: 6%;
			bottom: 10%;
			width: 170px;
		}

		.navigation-name {
			font-size: 11px;
		}

		.profile-frame {
			display: none;
		}

		.bottom-link {
			width: 210px;
		}

		.side-target {
			display: none;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.blink-dots,
		.status-bars span {
			animation: none;
		}

		.navigation-link {
			transition: none;
		}
	}
</style>
