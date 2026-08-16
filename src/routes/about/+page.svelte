<script lang="ts">
	import { onMount } from 'svelte';

	let namecard: HTMLImageElement | null = null;

	let targetX = 0;
	let targetY = 0;

	let currentX = 0;
	let currentY = 0;

	let animationFrame = 0;

	onMount(() => {
		const handleMouseMove = (event: MouseEvent) => {
			targetX =
				(event.clientX / window.innerWidth - 0.5) * 2;

			targetY =
				(event.clientY / window.innerHeight - 0.5) * 2;
		};

		const animate = () => {
			currentX +=
				(targetX - currentX) * 0.085;

			currentY +=
				(targetY - currentY) * 0.085;

			if (namecard) {
				const distance =
					Math.sqrt(
						currentX * currentX +
							currentY * currentY
					);

				const cursorStrength =
					Math.max(
						0,
						1 - distance
					);

				const scale =
					1.015 +
					cursorStrength * 0.035;

				const translateX =
					currentX * -8;

				const translateY =
					currentY * -6;

				namecard.style.transform = `
					translate3d(
						${translateX}px,
						${translateY}px,
						0
					)
					scale(${scale})
				`;
			}

			animationFrame =
				requestAnimationFrame(
					animate
				);
		};

		window.addEventListener(
			'mousemove',
			handleMouseMove,
			{ passive: true }
		);

		animationFrame =
			requestAnimationFrame(
				animate
			);

		return () => {
			window.removeEventListener(
				'mousemove',
				handleMouseMove
			);

			cancelAnimationFrame(
				animationFrame
			);
		};
	});
</script>

<svelte:head>
	<title>ANI — About</title>

	<meta
		name="description"
		content="ANI — Mechanical Engineering profile"
	/>
</svelte:head>

<div class="about-page">

	<!-- =================================================
	     NAMECARD
	     ================================================= -->

	<section class="namecard-section">

		<!-- Background/namecard image -->
		<div class="namecard-image-wrap">
			<img
				bind:this={namecard}
				class="namecard-image"
				src="/about-photo.png"
				alt="ANI profile"
			/>
		</div>

		<!-- =================================================
		     UX/UI FRAME OVERLAY

		     The PNG remains clean.
		     These are the live interface pieces.
		     ================================================= -->

		<div class="namecard-ui">

			<!-- TOP FRAME -->

			<div class="ui-top-line"></div>

			<div class="ui-top-left">
				<span class="ui-number">
					01
				</span>

				<span class="ui-label">
					SUBJECT / 01
				</span>
			</div>

			<div class="ui-top-right">
				PROFILE IMAGE
			</div>

			<!-- TOP CENTER NODE -->

			<div class="ui-top-node"></div>

			<!-- CORNERS -->

			<div class="ui-corner ui-corner-tl"></div>
			<div class="ui-corner ui-corner-tr"></div>
			<div class="ui-corner ui-corner-bl"></div>
			<div class="ui-corner ui-corner-br"></div>

			<!-- INNER FRAME -->

			<div class="ui-inner-frame"></div>

			<!-- LEFT SIDE DATA -->

			<div class="ui-side-data left-data">
				<div class="data-mark"></div>
				<div class="data-mark"></div>
				<div class="data-mark"></div>

				<div class="vertical-code">
					01 / ACTIVE
				</div>
			</div>

			<!-- RIGHT SIDE DATA -->

			<div class="ui-side-data right-data">
				<div class="data-mark"></div>
				<div class="data-mark"></div>
				<div class="data-mark"></div>

				<div class="vertical-code">
					VERIFIED
				</div>
			</div>

			<!-- BOTTOM FRAME -->

			<div class="ui-bottom-line"></div>

			<div class="ui-bottom-left">
				VITERBI / SCHOOL OF ENGINEERING
			</div>

			<div class="ui-bottom-right">
				PROFILE / VERIFIED
			</div>

			<!-- BOTTOM NODES -->

			<div class="ui-bottom-node left"></div>
			<div class="ui-bottom-node right"></div>

			<!-- SMALL DATA BARS -->

			<div class="ui-data-bars top-bars">
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
			</div>

			<div class="ui-data-bars bottom-bars">
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
				<span></span>
			</div>

		</div>
	</section>

</div>

<style>
	/* =====================================================
	   PAGE
	   ===================================================== */

	.about-page {
		position: relative;

		width: 100vw;
		height: 100vh;

		min-height: 100vh;

		overflow: hidden;

		background: #000;

		--pink: #ff0080;
		--pink-soft: rgba(255, 0, 128, 0.62);
		--pink-faint: rgba(255, 0, 128, 0.22);
	}

	/* =====================================================
	   NAMECARD SECTION

	   This determines the exact size of both the image
	   and the UX/UI frame.
	   ===================================================== */

	.namecard-section {
		position: absolute;

		left: 8%;
		top: 14%;

		width: 34%;
		height: 70%;

		min-width: 420px;
		min-height: 500px;

		z-index: 10;

		pointer-events: none;
	}

	/* =====================================================
	   NAMECARD IMAGE
	   ===================================================== */

	.namecard-image-wrap {
		position: absolute;

		inset: 0;

		overflow: hidden;

		background: #000;
	}

	.namecard-image {
		position: absolute;

		inset: -4%;

		width: 108%;
		height: 108%;

		object-fit: cover;

		object-position: center center;

		display: block;

		transform:
			translate3d(
				0,
				0,
				0
			)
			scale(1.015);

		will-change:
			transform;

		filter:
			brightness(0.98)
			contrast(1.04);

		user-select: none;
	}

	/* =====================================================
	   UX/UI LAYER
	   ===================================================== */

	.namecard-ui {
		position: absolute;

		inset: 0;

		z-index: 20;

		pointer-events: none;
	}

	/* =====================================================
	   MAIN OUTER FRAME
	   ===================================================== */

	.namecard-ui::before {
		content: '';

		position: absolute;

		inset: 0;

		border:
			1px solid
			var(--pink);

		box-shadow:
			0 0 8px
			rgba(
				255,
				0,
				128,
				0.08
			);

		animation:
			frame-glitch
			5s
			steps(3, end)
			infinite;
	}

	/* =====================================================
	   INNER FRAME
	   ===================================================== */

	.ui-inner-frame {
		position: absolute;

		left: 3.2%;
		right: 3.2%;

		top: 3.4%;
		bottom: 3.8%;

		border:
			1px solid
			var(--pink-soft);

		opacity: 0.7;

		animation:
			inner-glitch
			5.7s
			steps(3, end)
			infinite;
	}

	/* =====================================================
	   TOP LINE
	   ===================================================== */

	.ui-top-line {
		position: absolute;

		left: 2.7%;
		right: 2.7%;

		top: 6%;

		height: 1px;

		background:
			var(--pink-soft);
	}

	.ui-top-left {
		position: absolute;

		left: 3%;

		top: 1.8%;

		display: flex;

		align-items: center;

		gap: 7px;

		color:
			var(--pink);

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size:
			clamp(
				5px,
				0.42vw,
				8px
			);

		letter-spacing:
			0.08em;
	}

	.ui-number {
		color:
			var(--pink);

		font-weight: 700;
	}

	.ui-label {
		color:
			var(--pink-soft);
	}

	.ui-top-right {
		position: absolute;

		right: 3%;

		top: 1.8%;

		color:
			var(--pink-soft);

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size:
			clamp(
				5px,
				0.4vw,
				8px
			);

		letter-spacing:
			0.08em;
	}

	/* =====================================================
	   TOP NODE
	   ===================================================== */

	.ui-top-node {
		position: absolute;

		left: 50%;
		top: -4px;

		width: 8px;
		height: 8px;

		border:
			1px solid
			var(--pink);

		border-radius: 50%;

		background: #000;

		transform:
			translateX(-50%);

		animation:
			node-pulse
			2.5s
			ease-in-out
			infinite;
	}

	/* =====================================================
	   FRAME CORNERS
	   ===================================================== */

	.ui-corner {
		position: absolute;

		width: 18px;
		height: 18px;

		border-color:
			#fff;

		opacity: 0.8;

		z-index: 5;
	}

	.ui-corner-tl {
		left: -1px;
		top: -1px;

		border-left:
			1px solid;

		border-top:
			1px solid;
	}

	.ui-corner-tr {
		right: -1px;
		top: -1px;

		border-right:
			1px solid;

		border-top:
			1px solid;
	}

	.ui-corner-bl {
		left: -1px;
		bottom: -1px;

		border-left:
			1px solid;

		border-bottom:
			1px solid;
	}

	.ui-corner-br {
		right: -1px;
		bottom: -1px;

		border-right:
			1px solid;

		border-bottom:
			1px solid;
	}

	/* =====================================================
	   SIDE DATA
	   ===================================================== */

	.ui-side-data {
		position: absolute;

		top: 24%;

		bottom: 24%;

		width: 7px;

		display:
			flex;

		flex-direction:
			column;

		align-items:
			center;

		gap: 9px;
	}

	.left-data {
		left: 1.2%;
	}

	.right-data {
		right: 1.2%;
	}

	.data-mark {
		width: 4px;
		height: 4px;

		border:
			1px solid
			var(--pink);

		border-radius: 50%;

		animation:
			data-blink
			2s
			steps(3, end)
			infinite;
	}

	.vertical-code {
		margin-top: auto;

		writing-mode:
			vertical-rl;

		transform:
			rotate(180deg);

		color:
			var(--pink-soft);

		font-size: 5px;

		letter-spacing:
			0.12em;
	}

	/* =====================================================
	   BOTTOM LINE
	   ===================================================== */

	.ui-bottom-line {
		position: absolute;

		left: 2.7%;
		right: 2.7%;

		bottom: 6%;

		height: 1px;

		background:
			var(--pink-soft);
	}

	.ui-bottom-left,
	.ui-bottom-right {
		position: absolute;

		bottom: 1.8%;

		color:
			var(--pink-soft);

		font-family:
			'Courier New',
			Courier,
			monospace;

		font-size:
			clamp(
				5px,
				0.38vw,
				8px
			);

		letter-spacing:
			0.06em;
	}

	.ui-bottom-left {
		left: 3%;
	}

	.ui-bottom-right {
		right: 3%;
	}

	/* =====================================================
	   BOTTOM NODES
	   ===================================================== */

	.ui-bottom-node {
		position: absolute;

		bottom: -4px;

		width: 8px;
		height: 8px;

		border:
			1px solid
			var(--pink);

		border-radius: 50%;

		background: #000;

		animation:
			node-pulse
			2.8s
			ease-in-out
			infinite;
	}

	.ui-bottom-node.left {
		left: 50%;
		transform:
			translateX(
				calc(
					-50% - 18px
				)
			);
	}

	.ui-bottom-node.right {
		left: 50%;
		transform:
			translateX(
				calc(
					-50% + 18px
				)
			);
	}

	/* =====================================================
	   DATA BARS
	   ===================================================== */

	.ui-data-bars {
		position: absolute;

		display: flex;

		gap: 3px;
	}

	.ui-data-bars span {
		display: block;

		width: 6px;
		height: 4px;

		background:
			var(--pink-soft);

		animation:
			bar-glitch
			1.7s
			steps(4, end)
			infinite;
	}

	.top-bars {
		right: 8%;
		top: 10px;
	}

	.bottom-bars {
		left: 8%;
		bottom: 10px;
	}

	/* =====================================================
	   FRAME GLITCH
	   ===================================================== */

	@keyframes frame-glitch {
		0%,
		85%,
		100% {
			opacity: 1;

			transform:
				translate(
					0,
					0
				);
		}

		87% {
			opacity: 0.6;

			transform:
				translate(
					-1px,
					0
				);
		}

		89% {
			opacity: 0.95;

			transform:
				translate(
					2px,
					0
				);
		}

		92% {
			opacity: 0.7;

			transform:
				translate(
					-1px,
					1px
				);
		}

		94% {
			opacity: 1;

			transform:
				translate(
					0,
					0
				);
		}
	}

	@keyframes inner-glitch {
		0%,
		88%,
		100% {
			opacity: 0.7;
		}

		90% {
			opacity: 0.3;
		}

		92% {
			opacity: 0.9;
		}

		94% {
			opacity: 0.7;
		}
	}

	@keyframes node-pulse {
		0%,
		100% {
			opacity: 0.35;

			transform:
				translateX(-50%)
				scale(0.9);
		}

		50% {
			opacity: 1;

			transform:
				translateX(-50%)
				scale(1.15);
		}
	}

	.ui-bottom-node {
		animation:
			bottom-node-pulse
			2.8s
			ease-in-out
			infinite;
	}

	@keyframes bottom-node-pulse {
		0%,
		100% {
			opacity: 0.35;

			transform:
				scale(0.9);
		}

		50% {
			opacity: 1;

			transform:
				scale(1.15);
		}
	}

	@keyframes data-blink {
		0%,
		70%,
		100% {
			opacity: 0.35;
		}

		75% {
			opacity: 1;
		}

		80% {
			opacity: 0.18;
		}

		85% {
			opacity: 0.85;
		}
	}

	@keyframes bar-glitch {
		0%,
		70%,
		100% {
			opacity: 0.5;

			transform:
				scaleX(1);
		}

		74% {
			opacity: 0.15;

			transform:
				scaleX(0.6);
		}

		78% {
			opacity: 0.9;

			transform:
				scaleX(1.15);
		}

		82% {
			opacity: 0.3;

			transform:
				scaleX(0.8);
		}

		86% {
			opacity: 0.65;

			transform:
				scaleX(1);
		}
	}

	/* =====================================================
	   RESPONSIVE
	   ===================================================== */

	@media (max-width: 1100px) {
		.namecard-section {
			left: 7%;
			width: 42%;
		}
	}

	@media (max-width: 800px) {
		.namecard-section {
			left: 8%;
			top: 15%;

			width: 62%;
			height: 68%;

			min-width: 0;
			min-height: 0;
		}

		.ui-side-data {
			display: none;
		}
	}

	@media (max-width: 600px) {
		.namecard-section {
			left: 6%;
			top: 17%;

			width: 78%;
			height: 62%;
		}

		.ui-top-right,
		.ui-bottom-right {
			display: none;
		}

		.ui-data-bars {
			display: none;
		}
	}

	/* =====================================================
	   REDUCED MOTION
	   ===================================================== */

	@media (prefers-reduced-motion: reduce) {
		.namecard-image {
			transform:
				scale(1.015) !important;
		}

		.namecard-ui::before,
		.ui-inner-frame,
		.ui-top-node,
		.data-mark,
		.ui-data-bars span {
			animation: none;
		}
	}
</style>
