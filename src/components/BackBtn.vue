<template>
	<button :class="btnStyle" @click="btnEvent">
		<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 18 10">
			<title>down-triangle</title>
			<g id="Layer_2" data-name="Layer 2">
				<g id="Layer_1-2" data-name="Layer 1">
					<polygon class="cls-1" points="0 0 18 0 9 10 0 0" />
				</g>
			</g>
		</svg>
		<p>{{ content }}</p>
	</button>
</template>

<script>
	export default {
		name: "BackBtn",
		props: ["btnStyle", "content", "btnEvent"]
	};
</script>

<style lang="scss" scoped>
	button {
		border: none;
		border-radius: 30px;
		padding: 10px 20px;
		display: flex;
		align-items: center;

		p {
			font-size: 1.2em;
			margin: 0;
			text-transform: uppercase;
			font-family: $type-accent;
			letter-spacing: 0.1em;
		}

		svg {
			width: 10px;
			margin-right: 10px;
			transform: rotate(180deg);
		}

		&:hover {
			svg {
				animation: goUp 0.6s infinite;
			}
		}
	}

	@keyframes goUp {
		0% {
			transform: translateY(0) rotate(180deg);
		}
		50% {
			transform: translateY(-3px) rotate(180deg);
		}
		100% {
			transform: translateY(0) rotate(180deg);
		}
	}

	.back {
		background: $textGold;
		padding: 10px 15px;

		p {
			color: $pink;
			letter-spacing: 0.1em;
		}

		svg {
			fill: $pink;
		}
	}

	.restart {
		background: none;
		padding: 0;

		p {
			color: $textGold;
		}

		svg {
			width: 15px;
			fill: $textGold;
		}
	}
</style>
