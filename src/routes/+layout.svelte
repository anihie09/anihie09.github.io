<script lang="ts">
	import { onNavigate } from '$app/navigation';
	import type { Snippet } from 'svelte';

	let { children }: { children: Snippet } = $props();

	let glitching = $state(false);

	onNavigate((navigation) => {
		if (!navigation.from) {
			return;
		}

		glitching = true;

		return new Promise<void>((resolve) => {
			setTimeout(() => {
				resolve();

				setTimeout(() => {
					glitching = false;
				}, 260);
			}, 180);
		});
	});
</script>

<div class="site">
	{@render children()}

	{#if glitching}
		<div class="glitch-transition" aria-hidden="true">
			<div class="glitch-slice slice-one"></div>
			<div class="glitch-slice slice-two"></div>
			<div class="glitch-slice slice-three"></div>

			<div class="glitch-text">
				LOADING ///
			</div>
		</div>
	{/if}
</div>

<style>
	:global(html),
	:global(body) {
		margin: 0;
		padding: 0;
		background: #000;
	}

	:global(body) {
		overflow: hidden;
	}

	:global(*) {
		box-sizing: border-box;
	}

	.site {
		position: relative;
		width: 100%;
		min-height: 100vh;
		background: #000;
	}

	.glitch-transition {
		position: fixed;
		inset: 0;
		z-index: 9999;

		background: rgba(0, 0, 0, 0.94);

		pointer-events: none;
		overflow: hidden;

		animation: glitchIn 0.18s steps(3, end);
	}

	.glitch-slice {
		position: absolute;

		height: 5px;

		background: #e40000;

		animation: sliceMove 0.12s steps(3, end) infinite;
	}

	.slice-one {
		top: 28%;
		left: -5%;
		width: 45%;
	}

	.slice-two {
		top: 51%;
		right: -5%;
		width: 55%;
		animation-delay: 0.03s;
	}

	.slice-three {
		top: 73%;
		left: 15%;
		width: 35%;
		animation-delay: 0.06s;
	}

	.glitch-text {
		position: absolute;
		top: 50%;
		left: 50%;

		transform: translate(-50%, -50%);

		color: #fff;

		font-family:
			"Courier New",
			monospace;

		font-size: 0.8rem;
		font-weight: 700;
		letter-spacing: 0.16em;

		animation: textJitter 0.1s steps(2, end) infinite;
	}

	@keyframes glitchIn {
		0% {
			opacity: 0;
			transform: translateX(-8px);
		}

		45% {
			opacity: 1;
			transform: translateX(5px);
		}

		100% {
			opacity: 1;
			transform: translateX(0);
		}
	}

	@keyframes sliceMove {
		0%,
		100% {
			transform: translateX(0);
		}

		33% {
			transform: translateX(35px);
		}

		66% {
			transform: translateX(-20px);
		}
	}

	@keyframes textJitter {
		0%,
		100% {
			transform: translate(-50%, -50%);
		}

		50% {
			transform: translate(calc(-50% + 4px), -50%);
		}
	}
</style>
