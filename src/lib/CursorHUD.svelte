<script lang="ts">
	import { onMount } from 'svelte';

	let cursorX = $state(0);
	let cursorY = $state(0);

	let targetX = $state(0);
	let targetY = $state(0);

	let velocity = $state(0);
	let targetVelocity = $state(0);

	let rotation = $state(0);
	let targetRotation = $state(0);

	let visible = $state(false);

	let animationFrame = 0;

	/* =====================================================
	   DERIVED VALUES
	   Svelte 5 runes mode
	   ===================================================== */

	let intensity = $derived(
		Math.min(
			velocity / 24,
			1
		)
	);

	let scale = $derived(
		1 +
		intensity * 0.16
	);

	let stretch = $derived(
		1 +
		intensity * 0.24
	);

	let opacity = $derived(
		0.55 +
		intensity * 0.35
	);

	/* =====================================================
	   MOUSE / ANIMATION
	   ===================================================== */

	onMount(() => {
		const handleMouseMove = (
			event: MouseEvent
		) => {
			targetX = event.clientX;
			targetY = event.clientY;

			visible = true;

			/*
			 * Mouse movement amount.
			 */
			const dx = event.movementX;
			const dy = event.movementY;

			const movement =
				Math.sqrt(
					dx * dx +
					dy * dy
				);

			targetVelocity =
				Math.min(
					movement,
					24
				);

			/*
			 * Rotate the HUD according
			 * to the direction of movement.
			 */
			if (movement > 0.2) {
				targetRotation =
					Math.atan2(
						dy,
						dx
					) *
					(180 / Math.PI);
			}
		};

		const handleMouseLeave =
			() => {
				visible = false;
			};

		const animate = () => {
			/*
			 * Smooth cursor position.
			 */
			cursorX +=
				(targetX - cursorX) *
				0.16;

			cursorY +=
				(targetY - cursorY) *
				0.16;

			/*
			 * Smooth movement intensity.
			 */
			velocity +=
				(
					targetVelocity -
					velocity
				) *
				0.12;

			targetVelocity *= 0.89;

			/*
			 * Smooth rotation.
			 *
			 * This prevents the HUD from taking
			 * the long way around when crossing
			 * the -180 / 180 boundary.
			 */
			let rotationDifference =
				targetRotation -
				rotation;

			while (
				rotationDifference >
				180
			) {
				rotationDifference -=
					360;
			}

			while (
				rotationDifference <
				-180
			) {
				rotationDifference +=
					360;
			}

			rotation +=
				rotationDifference *
				0.12;

			animationFrame =
				requestAnimationFrame(
					animate
				);
		};

		window.addEventListener(
			'mousemove',
			handleMouseMove,
			{
				passive: true
			}
		);

		window.addEventListener(
			'mouseleave',
			handleMouseLeave
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

			window.removeEventListener(
				'mouseleave',
				handleMouseLeave
			);

			cancelAnimationFrame(
				animationFrame
			);
		};
	});
</script>

<div
	class:visible
	class="cursor-hud"
	style={`
		left: ${cursorX}px;
		top: ${cursorY}px;
		--intensity: ${intensity};
		--scale: ${scale};
		--stretch: ${stretch};
		--opacity: ${opacity};
		--rotation: ${rotation}deg;
	`}
>
	<!-- =================================================
	     CENTER POINT
	     ================================================= -->

	<div class="cursor-center">
		<div class="center-dot"></div>
	</div>

	<!-- =================================================
	     INNER DIAMOND
	     ================================================= -->

	<div class="cursor-diamond">
		<div class="diamond-line diamond-top"></div>
		<div class="diamond-line diamond-right"></div>
		<div class="diamond-line diamond-bottom"></div>
		<div class="diamond-line diamond-left"></div>

		<div class="diamond-node node-top"></div>
		<div class="diamond-node node-right"></div>
		<div class="diamond-node node-bottom"></div>
		<div class="diamond-node node-left"></div>
	</div>

	<!-- =================================================
	     OCTAGON
	     ================================================= -->

	<div class="cursor-octagon">
		<div class="octagon-inner"></div>

		<div class="octagon-cut cut-top"></div>
		<div class="octagon-cut cut-right"></div>
		<div class="octagon-cut cut-bottom"></div>
		<div class="octagon-cut cut-left"></div>
	</div>

	<!-- =================================================
	     OUTER BRACKETS
	     ================================================= -->

	<div class="cursor-bracket bracket-top">
		<span></span>
		<span></span>
	</div>

	<div class="cursor-bracket bracket-right">
		<span></span>
		<span></span>
	</div>

	<div class="cursor-bracket bracket-bottom">
		<span></span>
		<span></span>
	</div>

	<div class="cursor-bracket bracket-left">
		<span></span>
		<span></span>
	</div>

	<!-- =================================================
	     MICRO HUD MARKERS
	     ================================================= -->

	<div class="micro-marker marker-a"></div>
	<div class="micro-marker marker-b"></div>
	<div class="micro-marker marker-c"></div>
	<div class="micro-marker marker-d"></div>
</div>

<style>
	:global(:root) {
		--cursor-pink: #ff0080;
	}

	/* =====================================================
	   MAIN CURSOR HUD
	   ===================================================== */

	.cursor-hud {
		position: fixed;

		z-index: 9998;

		width: 0;
		height: 0;

		pointer-events: none;

		opacity: 0;

		will-change:
			transform,
			opacity;

		transform:
			translate3d(
				0,
				0,
				0
			);

		transition:
			opacity
			180ms
			ease;
	}

	.cursor-hud.visible {
		opacity:
			var(--opacity);
	}

	/* =====================================================
	   CENTER
	   ===================================================== */

	.cursor-center {
		position: absolute;

		left: 0;
		top: 0;

		width: 8px;
		height: 8px;

		transform:
			translate(
				-50%,
				-50%
			);

		display:
			flex;

		align-items:
			center;

		justify-content:
			center;
	}

	.center-dot {
		width: 3px;
		height: 3px;

		border-radius: 50%;

		background:
			var(--cursor-pink);

		box-shadow:
			0 0 4px
			rgba(
				255,
				0,
				128,
				0.7
			);

		animation:
			center-pulse
			1.7s
			ease-in-out
			infinite;
	}

	/* =====================================================
	   INNER DIAMOND
	   ===================================================== */

	.cursor-diamond {
		position: absolute;

		left: 0;
		top: 0;

		width: 46px;
		height: 46px;

		transform:
			translate(
				-50%,
				-50%
			)
			rotate(45deg)
			scale(
				var(--scale)
			);

		transition:
			transform
			80ms
			linear;
	}

	.diamond-line {
		position: absolute;

		background:
			var(--cursor-pink);

		opacity: 0.72;
	}

	.diamond-top,
	.diamond-bottom {
		width: 15px;
		height: 1px;

		left: 50%;
	}

	.diamond-top {
		top: 1px;

		transform:
			translateX(
				-50%
			);
	}

	.diamond-bottom {
		bottom: 1px;

		transform:
			translateX(
				-50%
			);
	}

	.diamond-left,
	.diamond-right {
		width: 1px;
		height: 15px;

		top: 50%;
	}

	.diamond-left {
		left: 1px;

		transform:
			translateY(
				-50%
			);
	}

	.diamond-right {
		right: 1px;

		transform:
			translateY(
				-50%
			);
	}

	.diamond-node {
		position: absolute;

		width: 4px;
		height: 4px;

		border:
			1px solid
			var(--cursor-pink);

		background:
			#000;
	}

	.node-top {
		left: 50%;
		top: -2px;

		transform:
			translateX(
				-50%
			);
	}

	.node-right {
		right: -2px;
		top: 50%;

		transform:
			translateY(
				-50%
			);
	}

	.node-bottom {
		left: 50%;
		bottom: -2px;

		transform:
			translateX(
				-50%
			);
	}

	.node-left {
		left: -2px;
		top: 50%;

		transform:
			translateY(
				-50%
			);
	}

	/* =====================================================
	   OCTAGON
	   ===================================================== */

	.cursor-octagon {
		position: absolute;

		left: 0;
		top: 0;

		width: 82px;
		height: 82px;

		transform:
			translate(
				-50%,
				-50%
			)
			scale(
				var(--scale)
			);

		border:
			1px solid
			rgba(
				255,
				0,
				128,
				0.43
			);

		clip-path:
			polygon(
				30% 0%,
				70% 0%,
				100% 30%,
				100% 70%,
				70% 100%,
				30% 100%,
				0% 70%,
				0% 30%
			);

		transition:
			transform
			90ms
			ease-out;
	}

	.octagon-inner {
		position: absolute;

		left: 50%;
		top: 50%;

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
				0.18
			);

		clip-path:
			polygon(
				30% 0%,
				70% 0%,
				100% 30%,
				100% 70%,
				70% 100%,
				30% 100%,
				0% 70%,
				0% 30%
			);
	}

	.octagon-cut {
		position: absolute;

		background:
			var(--cursor-pink);

		opacity: 0.82;
	}

	.cut-top,
	.cut-bottom {
		width: 16px;
		height: 1px;

		left: 50%;
	}

	.cut-top {
		top: 0;

		transform:
			translateX(
				-50%
			);
	}

	.cut-bottom {
		bottom: 0;

		transform:
			translateX(
				-50%
			);
	}

	.cut-left,
	.cut-right {
		width: 1px;
		height: 16px;

		top: 50%;
	}

	.cut-left {
		left: 0;

		transform:
			translateY(
				-50%
			);
	}

	.cut-right {
		right: 0;

		transform:
			translateY(
				-50%
			);
	}

	/* =====================================================
	   OUTER BRACKETS
	   ===================================================== */

	.cursor-bracket {
		position: absolute;

		left: 0;
		top: 0;

		width: 118px;
		height: 118px;

		transform:
			translate(
				-50%,
				-50%
			)
			scaleX(
				var(--stretch)
			);

		transition:
			transform
			100ms
			ease-out;
	}

	.cursor-bracket span {
		position: absolute;

		display: block;

		background:
			var(--cursor-pink);

		opacity:
			calc(
				0.25 +
				var(--intensity) * 0.25
			);
	}

	/* TOP */

	.bracket-top span:first-child {
		left: 30%;
		top: 3px;

		width: 20px;
		height: 1px;
	}

	.bracket-top span:last-child {
		right: 30%;
		top: 3px;

		width: 20px;
		height: 1px;
	}

	/* RIGHT */

	.bracket-right span:first-child {
		right: 3px;
		top: 30%;

		width: 1px;
		height: 20px;
	}

	.bracket-right span:last-child {
		right: 3px;
		bottom: 30%;

		width: 1px;
		height: 20px;
	}

	/* BOTTOM */

	.bracket-bottom span:first-child {
		left: 30%;
		bottom: 3px;

		width: 20px;
		height: 1px;
	}

	.bracket-bottom span:last-child {
		right: 30%;
		bottom: 3px;

		width: 20px;
		height: 1px;
	}

	/* LEFT */

	.bracket-left span:first-child {
		left: 3px;
		top: 30%;

		width: 1px;
		height: 20px;
	}

	.bracket-left span:last-child {
		left: 3px;
		bottom: 30%;

		width: 1px;
		height: 20px;
	}

	/* =====================================================
	   MICRO MARKERS
	   ===================================================== */

	.micro-marker {
		position: absolute;

		left: 0;
		top: 0;

		width: 4px;
		height: 4px;

		border:
			1px solid
			var(--cursor-pink);

		background:
			#000;

		opacity: 0.52;
	}

	.marker-a {
		transform:
			translate(
				-2px,
				-60px
			);
	}

	.marker-b {
		transform:
			translate(
				56px,
				-2px
			);
	}

	.marker-c {
		transform:
			translate(
				-2px,
				56px
			);
	}

	.marker-d {
		transform:
			translate(
				-60px,
				-2px
			);
	}

	/* =====================================================
	   ANIMATION
	   ===================================================== */

	@keyframes center-pulse {
		0%,
		100% {
			opacity: 0.45;

			transform:
				scale(0.8);
		}

		50% {
			opacity: 1;

			transform:
				scale(1.25);
		}
	}

	/* =====================================================
	   REDUCED MOTION
	   ===================================================== */

	@media (
		prefers-reduced-motion: reduce
	) {
		.cursor-hud {
			display: none;
		}
	}

	/* =====================================================
	   TOUCH DEVICES
	   ===================================================== */

	@media (hover: none) {
		.cursor-hud {
			display: none;
		}
	}
</style>
