<template>
	<section id="wrapper">
		<header>
			<h2>{{ newYearsValue }}</h2>
			<BottomRidge />
			<BottomRidgeDesktop />
		</header>
		<div id="home">
			<SDGBug page="home" />
			<HeroIllustration />
			<div class="home-content">
				<h1 class="home-title uppercase text-center">Generator</h1>
				<p class="home-subtitle">
					Find out what kind of noodles you’d be in 15 questions. Live long and
					noodle on.
				</p>
				<button class="start-quiz-btn" @click="startQuiz" v-if="!quizStarted">
					<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 18 10">
						<title>down-triangle</title>
						<g id="Layer_2" data-name="Layer 2">
							<g id="Layer_1-2" data-name="Layer 1">
								<polygon class="cls-1" points="0 0 18 0 9 10 0 0" />
							</g>
						</g>
					</svg>
					<p>take the quiz</p>
				</button>
			</div>
			<BottomLeft />
			<TopRight />
		</div>
	</section>
</template>

<script>
	import HeroIllustration from "./Illustrations/HeroIllustration";
	import BottomLeft from "./Illustrations/BottomLeft";
	import TopRight from "./Illustrations/TopRight";
	import BottomRidge from "./Illustrations/BottomRidge";
	import BottomRidgeDesktop from "./Illustrations/BottomRidgeDesktop";
	import SDGBug from "./Illustrations/SDGBug";

	export default {
		name: "Landing",
		components: {
			HeroIllustration,
			BottomRidge,
			BottomRidgeDesktop,
			BottomLeft,
			TopRight,
			SDGBug
		},
		props: ["quizStarted"],
		data() {
			return {
				newYearsValue: "KUNG HEI FAT CHOY!"
			};
		},
		mounted() {
			setInterval(() => {
				if (this.newYearsValue === "KUNG HEI FAT CHOY!") {
					this.newYearsValue = "HAPPY NEW YEAR!";
				} else {
					this.newYearsValue = "KUNG HEI FAT CHOY!";
				}
			}, 1500);
		},
		methods: {
			startQuiz() {
				this.$store.commit("setQuizStarted", {
					value: true
				});
			}
		}
	};
</script>

<style lang="scss" scoped>
	#home {
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		margin-top: 5vh;
		height: 95vh;
		background: $pink;

		.home-content {
			position: absolute;
			z-index: 10;
			top: 50%;
			text-align: center;
			z-index: 99999999;
		}
	}

	header {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		color: $pink;
		background: $white;
		height: 5vh;
		display: flex;
		align-items: center;
		justify-content: center;

		h2 {
			position: relative;
			z-index: 7;
			font-size: 1rem;
			font-weight: 900;
			top: 17.5%;
		}

		svg {
			position: absolute;
			bottom: -35%;
			left: 0;
			z-index: 1;
			min-width: 100%;
		}
	}

	.home-title {
		margin: 0;
		font-size: 8vmin;
		transition: all 0.3s $cubic-ease;
		position: relative;
		color: $white;
		text-align: center;
		font-family: $type-accent;
		position: relative;
		letter-spacing: 0.02em;
	}

	p {
		line-height: 1.3;
	}

	.home-subtitle {
		text-align: center;
		font-size: 16px;
		font-weight: bold;
		color: $white;
		margin-bottom: 30px;
		font-weight: 800;
		margin-top: 20px;
		max-width: 100%;
		padding: 0 10px;
	}

	.start-quiz-btn {
		border: none;
		margin: 1rem auto 0 auto;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		border-radius: 40px;
		background: #70c4ba;
		padding: 10px 20px;
		text-transform: uppercase;
		font-size: 1em;
		letter-spacing: 0.1em;
		transition: all 0.3s ease-out;

		p {
			font-weight: 700;
			color: $white;
			margin: 0;
		}

		&:hover {
			transition: all 0.3s ease-in;
			background: #92d6ca;

			svg {
				animation: upDown 0.6s ease infinite;
			}
		}

		svg {
			width: 10px;
			margin-right: 10px;

			.cls-1 {
				fill: $white;
			}
		}
	}

	@media (min-width: $breakpoint-small) {
		header {
			h2 {
				letter-spacing: 0.1em;
				font-size: 1.5rem;
				top: 35%;
			}

			svg {
				bottom: -50%;
			}
		}

		.home-subtitle {
			font-size: 24px;
		}

		.start-quiz-btn {
			padding: 13px 30px;
			font-size: 1.5em;
		}
	}

	@keyframes upDown {
		0% {
			transform: translateY(0);
		}
		50% {
			transform: translateY(3px);
		}
		100% {
			transform: translateY(0);
		}
	}
</style>
