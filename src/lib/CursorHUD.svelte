<script lang="ts">
	import { onMount } from 'svelte';

	let canvas: HTMLCanvasElement;

	let mouseX = $state(0);
	let mouseY = $state(0);

	let targetX = $state(0);
	let targetY = $state(0);

	let velocity = $state(0);
	let targetVelocity = $state(0);

	let visible = $state(false);

	onMount(() => {
		const ctx = canvas.getContext('2d');

		if (!ctx) {
			return;
		}

		let width = window.innerWidth;
		let height = window.innerHeight;

		const characters =
			'01<>[]{}+*#%@';

		const particles: Array<{
			x: number;
			y: number;
			character: string;
			alpha: number;
			size: number;
			offsetX: number;
			offsetY: number;
			baseAlpha: number;
		}> = [];

		const resize = () => {
			width =
				window.innerWidth;

			height =
				window.innerHeight;

			const dpr =
				Math.min(
					window.devicePixelRatio || 1,
					2
				);

			canvas.width =
				width * dpr;

			canvas.height =
				height * dpr;

			canvas.style.width =
				`${width}px`;

			canvas.style.height =
				`${height}px`;

			ctx.setTransform(
				dpr,
				0,
				0,
				dpr,
				0,
				0
			);
		};

		const generateParticles = () => {
			particles.length = 0;

			const count =
				Math.min(
					700,
					Math.floor(
						(width * height) /
							14000
					)
				);

			for (
				let i = 0;
				i < count;
				i++
			) {
				const alpha =
					0.055 +
					Math.random() *
						0.24;

				particles.push({
					x:
						Math.random() *
						width,

					y:
						Math.random() *
						height,

					character:
						characters[
							Math.floor(
								Math.random() *
									characters.length
							)
						],

					alpha,

					baseAlpha:
						alpha,

					size:
						7 +
						Math.random() *
							4,

					offsetX: 0,
					offsetY: 0
				});
			}
		};

		resize();
		generateParticles();

		const handleResize = () => {
			resize();
			generateParticles();
		};

		const handleMove = (
			event: MouseEvent
		) => {
			const nextX =
				event.clientX;

			const nextY =
				event.clientY;

			const dx =
				nextX -
				targetX;

			const dy =
				nextY -
				targetY;

			targetVelocity =
				Math.sqrt(
					dx * dx +
						dy * dy
				);

			targetX = nextX;
			targetY = nextY;

			if (!visible) {
				mouseX = nextX;
				mouseY = nextY;
			}

			visible = true;
		};

		const handleLeave = () => {
			visible = false;
		};

		window.addEventListener(
			'resize',
			handleResize
		);

		window.addEventListener(
			'mousemove',
			handleMove
		);

		window.addEventListener(
			'mouseleave',
			handleLeave
		);

		let animationFrame = 0;

		const render = (
			time: number
		) => {
			mouseX +=
				(targetX - mouseX) *
				0.18;

			mouseY +=
				(targetY - mouseY) *
				0.18;

			velocity +=
				(targetVelocity -
					velocity) *
				0.2;

			targetVelocity *=
				0.84;

			ctx.clearRect(
				0,
				0,
				width,
				height
			);

			if (visible) {
				const speed =
					Math.min(
						velocity / 18,
						1
					);

				const radius =
					96 +
					speed * 22;

				/* =========================================
				   SOFT CIRCULAR FIELD
				   ========================================= */

				const gradient =
					ctx.createRadialGradient(
						mouseX,
						mouseY,
						0,
						mouseX,
						mouseY,
						radius
					);

				gradient.addColorStop(
					0,
					'rgba(255,0,128,0.075)'
				);

				gradient.addColorStop(
					0.3,
					'rgba(255,0,128,0.045)'
				);

				gradient.addColorStop(
					0.62,
					'rgba(255,0,128,0.018)'
				);

				gradient.addColorStop(
					1,
					'rgba(255,0,128,0)'
				);

				ctx.fillStyle =
					gradient;

				ctx.beginPath();

				ctx.arc(
					mouseX,
					mouseY,
					radius,
					0,
					Math.PI * 2
				);

				ctx.fill();

				/* =========================================
				   TEXTURE FIELD
				   ========================================= */

				ctx.font =
					'8px monospace';

				ctx.textAlign =
					'center';

				ctx.textBaseline =
					'middle';

				for (
					let i = 0;
					i <
					particles.length;
					i++
				) {
					const particle =
						particles[i];

					const dx =
						particle.x -
						mouseX;

					const dy =
						particle.y -
						mouseY;

					const distance =
						Math.sqrt(
							dx * dx +
								dy * dy
						);

					if (
						distance >
						radius
					) {
						continue;
					}

					const influence =
						Math.max(
							0,
							1 -
								distance /
									radius
						);

					const strength =
						Math.pow(
							influence,
							2.4
						);

					const angle =
						Math.atan2(
							dy,
							dx
						);

					const ripple =
						Math.sin(
							distance *
								0.13 -
								time *
									0.002
						);

					const displacement =
						strength *
						(4 +
							speed *
								7);

					particle.offsetX =
						Math.cos(
							angle
						) *
						ripple *
						displacement;

					particle.offsetY =
						Math.sin(
							angle
						) *
						ripple *
						displacement;

					/*
					 * Strongest directly under
					 * the pointer, then gradually
					 * dissolves into the page.
					 */

					const alpha =
						particle.baseAlpha *
						(0.15 +
							strength *
								1.25);

					const red = 255;

					const green =
						Math.floor(
							60 +
								100 *
									(1 -
										strength)
						);

					const blue =
						140 +
						Math.floor(
							40 *
								(1 -
									strength)
						);

					ctx.fillStyle =
						`rgba(${red},${green},${blue},${Math.min(alpha, 0.42)})`;

					ctx.fillText(
						particle.character,
						particle.x +
							particle.offsetX,
						particle.y +
							particle.offsetY
					);
				}

				/* =========================================
				   CURSOR MICRO HUD
				   ========================================= */

				const micro =
					13 +
					speed * 4;

				ctx.strokeStyle =
					`rgba(255,0,128,${0.28 + speed * 0.16})`;

				ctx.lineWidth = 1;

				ctx.beginPath();

				ctx.arc(
					mouseX,
					mouseY,
					micro,
					0,
					Math.PI * 2
				);

				ctx.stroke();

				/* four small directional marks */

				for (
					let i = 0;
					i < 4;
					i++
				) {
					const angle =
						(i *
							Math.PI) /
						2;

					const inner =
						micro + 5;

					const outer =
						micro + 9;

					ctx.beginPath();

					ctx.moveTo(
						mouseX +
							Math.cos(
								angle
							) *
								inner,

						mouseY +
							Math.sin(
								angle
							) *
								inner
					);

					ctx.lineTo(
						mouseX +
							Math.cos(
								angle
							) *
								outer,

						mouseY +
							Math.sin(
								angle
							) *
								outer
					);

					ctx.stroke();
				}
			}

			animationFrame =
				requestAnimationFrame(
					render
				);
		};

		animationFrame =
			requestAnimationFrame(
				render
			);

		return () => {
			window.removeEventListener(
				'resize',
				handleResize
			);

			window.removeEventListener(
				'mousemove',
				handleMove
			);

			window.removeEventListener(
				'mouseleave',
				handleLeave
			);

			cancelAnimationFrame(
				animationFrame
			);
		};
	});
</script>

<canvas
	bind:this={canvas}
	class="cursor-hud"
	aria-hidden="true"
></canvas>

<style>
	.cursor-hud {
		position: fixed;

		inset: 0;

		z-index: 85;

		width: 100vw;
		height: 100vh;

		pointer-events: none;

		mix-blend-mode: screen;
	}
</style>
