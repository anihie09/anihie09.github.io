<script lang="ts">
	import { onMount } from 'svelte';

	let canvas: HTMLCanvasElement;

	let {
		cellSize = 5,
		mouseRadius = 145,
		strength = 1.15,
		maxZoom = 1.055
	}: {
		cellSize?: number;
		mouseRadius?: number;
		strength?: number;
		maxZoom?: number;
	} = $props();

	onMount(() => {
		const context = canvas.getContext('2d');

		if (!context) {
			return;
		}

		const ctx: CanvasRenderingContext2D = context;

		let width = window.innerWidth;
		let height = window.innerHeight;

		let mouseX = -1000;
		let mouseY = -1000;

		let targetX = -1000;
		let targetY = -1000;

		let image: HTMLImageElement | null = null;
		let sourceCanvas: HTMLCanvasElement | null = null;
		let sourceContext: CanvasRenderingContext2D | null = null;

		let animationFrame = 0;

		const resize = () => {
			width = window.innerWidth;
			height = window.innerHeight;

			const dpr = Math.min(
				window.devicePixelRatio || 1,
				2
			);

			canvas.width = width * dpr;
			canvas.height = height * dpr;

			canvas.style.width = `${width}px`;
			canvas.style.height = `${height}px`;

			ctx.setTransform(
				dpr,
				0,
				0,
				dpr,
				0,
				0
			);

			createSourceBuffer();
		};

		const getImageBounds = () => {
			if (!image) {
				return {
					width: 0,
					height: 0,
					x: 0,
					y: 0
				};
			}

			const imageRatio =
				image.naturalWidth /
				image.naturalHeight;

			const screenRatio =
				width / height;

			let drawWidth = width;
			let drawHeight = height;
			let x = 0;
			let y = 0;

			/*
			 * Preserve the photograph's
			 * original aspect ratio.
			 */
			if (imageRatio > screenRatio) {
				drawWidth = width;
				drawHeight =
					width / imageRatio;

				y =
					(height - drawHeight) /
					2;
			} else {
				drawHeight = height;
				drawWidth =
					height * imageRatio;

				x =
					(width - drawWidth) /
					2;
			}

			return {
				width: drawWidth,
				height: drawHeight,
				x,
				y
			};
		};

		const createSourceBuffer = () => {
			if (!image || !image.complete) {
				return;
			}

			const buffer =
				document.createElement(
					'canvas'
				);

			buffer.width = width;
			buffer.height = height;

			const bufferContext =
				buffer.getContext('2d');

			if (!bufferContext) {
				return;
			}

			const bounds =
				getImageBounds();

			bufferContext.clearRect(
				0,
				0,
				width,
				height
			);

			bufferContext.drawImage(
				image,
				bounds.x,
				bounds.y,
				bounds.width,
				bounds.height
			);

			sourceCanvas = buffer;
			sourceContext = bufferContext;
		};

		const pointerMove = (
			event: PointerEvent
		) => {
			targetX = event.clientX;
			targetY = event.clientY;
		};

		const pointerLeave = () => {
			targetX = -1000;
			targetY = -1000;
		};

		const drawSoftZoom = (
			bounds: ReturnType<
				typeof getImageBounds
			>
		) => {
			if (
				!image ||
				!sourceCanvas
			) {
				return;
			}

			/*
			 * Distance from cursor to each
			 * point will determine zoom strength.
			 *
			 * The maximum remains 5.5%.
			 */
			const radius =
				mouseRadius * 2.35;

			const zoomCanvas =
				document.createElement(
					'canvas'
				);

			zoomCanvas.width = width;
			zoomCanvas.height = height;

			const zoomContext =
				zoomCanvas.getContext('2d');

			if (!zoomContext) {
				return;
			}

			/*
			 * Draw the zoomed version around
			 * the cursor.
			 */
			const scale = maxZoom;

			const zoomedWidth =
				bounds.width * scale;

			const zoomedHeight =
				bounds.height * scale;

			const zoomedX =
				mouseX -
				(mouseX - bounds.x) *
					scale;

			const zoomedY =
				mouseY -
				(mouseY - bounds.y) *
					scale;

			zoomContext.drawImage(
				image,
				zoomedX,
				zoomedY,
				zoomedWidth,
				zoomedHeight
			);

			/*
			 * Soft radial alpha mask.
			 *
			 * There is deliberately NO hard
			 * circular edge.
			 */
			zoomContext.globalCompositeOperation =
				'destination-in';

			const gradient =
				zoomContext.createRadialGradient(
					mouseX,
					mouseY,
					0,
					mouseX,
					mouseY,
					radius
				);

			gradient.addColorStop(
				0,
				'rgba(255,255,255,0.88)'
			);

			gradient.addColorStop(
				0.18,
				'rgba(255,255,255,0.72)'
			);

			gradient.addColorStop(
				0.42,
				'rgba(255,255,255,0.38)'
			);

			gradient.addColorStop(
				0.68,
				'rgba(255,255,255,0.12)'
			);

			gradient.addColorStop(
				1,
				'rgba(255,255,255,0)'
			);

			zoomContext.fillStyle =
				gradient;

			zoomContext.fillRect(
				0,
				0,
				width,
				height
			);

			zoomContext.globalCompositeOperation =
				'source-over';

			/*
			 * Blend the zoom smoothly over
			 * the original photograph.
			 */
			ctx.drawImage(
				zoomCanvas,
				0,
				0
			);
		};

		const drawHalftoneField = () => {
			if (
				!sourceContext ||
				!sourceCanvas ||
				mouseX < -500 ||
				mouseY < -500
			) {
				return;
			}

			/*
			 * Larger than the zoom radius so
			 * the halftone fades out gradually.
			 */
			const radius =
				mouseRadius * 1.9;

			const left = Math.max(
				0,
				mouseX - radius
			);

			const right = Math.min(
				width,
				mouseX + radius
			);

			const top = Math.max(
				0,
				mouseY - radius
			);

			const bottom = Math.min(
				height,
				mouseY + radius
			);

			const pixels =
				sourceContext.getImageData(
					0,
					0,
					width,
					height
				).data;

			const characters =
				' .:-=+*#%@';

			ctx.font =
				`${cellSize}px monospace`;

			ctx.textAlign = 'center';
			ctx.textBaseline = 'middle';

			const time =
				performance.now() /
				1000;

			for (
				let y = top;
				y < bottom;
				y += cellSize
			) {
				for (
					let x = left;
					x < right;
					x += cellSize
				) {
					const dx =
						x - mouseX;

					const dy =
						y - mouseY;

					const distance =
						Math.sqrt(
							dx * dx +
								dy * dy
						);

					if (distance > radius) {
						continue;
					}

					/*
					 * Smooth radial falloff.
					 *
					 * 1.0 directly underneath
					 * the cursor.
					 */
					const normalized =
						1 -
						Math.min(
							1,
							distance / radius
						);

					const influence =
						normalized *
						normalized;

					if (influence < 0.025) {
						continue;
					}

					const px = Math.min(
						width - 1,
						Math.max(
							0,
							Math.floor(x)
						)
					);

					const py = Math.min(
						height - 1,
						Math.max(
							0,
							Math.floor(y)
						)
					);

					const index =
						(py * width + px) *
						4;

					const red =
						pixels[index];

					const green =
						pixels[index + 1];

					const blue =
						pixels[index + 2];

					const brightness =
						0.299 * red +
						0.587 * green +
						0.114 * blue;

					const characterIndex =
						Math.min(
							characters.length - 1,
							Math.floor(
								(brightness /
									255) *
									(characters.length -
										1)
							)
						);

					const character =
						characters[
							characterIndex
						];

					if (
						character === ' '
					) {
						continue;
					}

					/*
					 * Distortion is strongest at
					 * the cursor and gradually
					 * weakens outward.
					 */
					const waveX =
						Math.sin(
							time * 6 +
								y * 0.04
						);

					const waveY =
						Math.cos(
							time * 5 +
								x * 0.035
						);

					const offsetX =
						waveX *
						influence *
						strength *
						6;

					const offsetY =
						waveY *
						influence *
						strength *
						2.5;

					/*
					 * Very subtle red tint.
					 */
					const greenValue =
						Math.round(
							255 -
								influence *
									105
						);

					const blueValue =
						Math.round(
							255 -
								influence *
									105
						);

					const alpha =
						0.04 +
						influence *
							0.76;

					ctx.fillStyle =
						`rgba(255,${greenValue},${blueValue},${alpha})`;

					ctx.fillText(
						character,
						x +
							cellSize / 2 +
							offsetX,
						y +
							cellSize / 2 +
							offsetY
					);
				}
			}
		};

		const drawBackgroundBlend = () => {
			if (
				mouseX < -500 ||
				mouseY < -500
			) {
				return;
			}

			/*
			 * A very soft red/black atmospheric
			 * field helps the zoom and halftone
			 * feel like part of the photograph.
			 */
			const radius =
				mouseRadius * 2.7;

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
				'rgba(180,0,0,0.055)'
			);

			gradient.addColorStop(
				0.25,
				'rgba(110,0,0,0.04)'
			);

			gradient.addColorStop(
				0.55,
				'rgba(45,0,0,0.018)'
			);

			gradient.addColorStop(
				1,
				'rgba(0,0,0,0)'
			);

			ctx.fillStyle =
				gradient;

			ctx.fillRect(
				0,
				0,
				width,
				height
			);
		};

		const animate = () => {
			/*
			 * Smooth cursor response.
			 */
			mouseX +=
				(targetX - mouseX) *
				0.12;

			mouseY +=
				(targetY - mouseY) *
				0.12;

			ctx.clearRect(
				0,
				0,
				width,
				height
			);

			if (
				image &&
				sourceCanvas
			) {
				const bounds =
					getImageBounds();

				/*
				 * 1. Soft background blend.
				 */
				drawBackgroundBlend();

				/*
				 * 2. Zoomed image with
				 * a feathered mask.
				 */
				if (
					mouseX > -500 &&
					mouseY > -500
				) {
					drawSoftZoom(bounds);

					/*
					 * 3. Halftone and subtle
					 * local distortion.
					 */
					drawHalftoneField();
				}
			}

			animationFrame =
				requestAnimationFrame(
					animate
				);
		};

		image = new Image();

		image.src =
			'/home-background.png';

		image.onload = () => {
			resize();
			animate();
		};

		window.addEventListener(
			'resize',
			resize
		);

		window.addEventListener(
			'pointermove',
			pointerMove
		);

		window.addEventListener(
			'pointerleave',
			pointerLeave
		);

		return () => {
			cancelAnimationFrame(
				animationFrame
			);

			window.removeEventListener(
				'resize',
				resize
			);

			window.removeEventListener(
				'pointermove',
				pointerMove
			);

			window.removeEventListener(
				'pointerleave',
				pointerLeave
			);
		};
	});
</script>

<canvas
	bind:this={canvas}
	class="ascii-overlay"
	aria-hidden="true"
></canvas>

<style>
	.ascii-overlay {
		position: fixed;
		inset: 0;

		z-index: 5;

		width: 100vw;
		height: 100vh;

		display: block;

		pointer-events: none;
	}
</style>
