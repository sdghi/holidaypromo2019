<template>
	<section class="results-container" :class="moodClass">
		<SDGBug page="results" />
		<div id="info-container">
			<YouAreSvg />
			<div class="ramen-info" v-if="topping.refId === 2">
				<h2 class="ramen-title">a Disenchanted Underachiever</h2>
				<p>
					<strong>Instant Ramen: </strong>minimal effort. Fill to the line, or
					just eat it crunchy.
				</p>
				<p>
					When presented with a choice, you reliably choose the easier.
					Immediate gratification is routinely sought at the expense of basic
					health and hygiene. Maybe try a little harder?
				</p>
				<p class="quote">“Let’s not do anything today.”</p>
			</div>
			<div class="ramen-info" v-else-if="topping.refId !== 2">
				<h2 class="ramen-title">
					{{ topping.starter }} {{ topping.name }}, {{ bowl.name }}
					{{ brothAndNoodle.name }}.
				</h2>
				<div class="description-container">
					<p>
						<strong>{{ brothAndNoodle.title }}: </strong>
						{{ brothAndNoodle.type }}
					</p>
					<p>{{ brothAndNoodle.description }}</p>
					<p class="quote">{{ topping.description }}</p>
				</div>
			</div>
			<p class="share-cta">
				🍜<strong>SEND NOODS!</strong> Screenshot and share your results on
				Instagram with the hashtag
				<strong class="insta-hashtag">#saemingenerator</strong>.
			</p>
			<BackBtn
				content="try again!"
				btnStyle="restart"
				:btnEvent="restartQuiz"
			/>
		</div>
		<Ramen
			:brothAndNoodleRef="brothAndNoodle.refId"
			:toppingRef="topping.refId"
			:bowlRef="bowl.refId"
			:moodClass="moodClass"
		/>
		<audio ref="burpAudio" src="burp.mp3"></audio>
		<BottomLeft status="blue" />
		<TopRight status="blue" />
	</section>
</template>

<script>
	import { mapGetters } from "vuex";
	import Ramen from "./Ramen";
	import BottomLeft from "./Illustrations/BottomLeft";
	import TopRight from "./Illustrations/TopRight";
	import BackBtn from "./BackBtn";
	import YouAreSvg from "./Illustrations/YouAreSvg";
	import SDGBug from "./Illustrations/SDGBug";

	export default {
		name: "Results",
		components: {
			SDGBug,
			BottomLeft,
			TopRight,
			Ramen,
			BackBtn,
			YouAreSvg
		},
		computed: mapGetters([
			"allIngredients",
			"allQuestions",
			"noodlesAndBroth",
			"allNoodlesAndBroth",
			"toppings",
			"allToppings",
			"bowls",
			"allBowls",
			"getAudioStatus"
		]),
		data() {
			return {
				brothAndNoodle: null,
				topping: null,
				bowl: null,
				questionCardHeight: null,
				moodClass: ""
			};
		},
		mounted() {
			// Play the burp sound
			this.playAudio();

			// Creates mood class for svgs based off of the topping value
			let toppingVal = this.topping.refId;

			switch (toppingVal) {
				case 0:
					this.moodClass = "optimistic";
					break;
				case 1:
					this.moodClass = "troubled";
					break;
				case 2:
					this.moodClass = "disenchanted";
					break;
				case 3:
					this.moodClass = "anxious";
					break;
				case 4:
					this.moodClass = "easygoing";
					break;
				case 5:
					this.moodClass = "angry";
					break;
				default:
					break;
			}
		},
		created() {
			this.brothAndNoodle = this.getIngredientValue(
				"brothAndNoodle",
				this.allNoodlesAndBroth
			);

			this.topping = this.getIngredientValue("toppings", this.allToppings);

			this.bowl = this.getIngredientValue("bowl", this.allBowls);
		},
		methods: {
			playAudio() {
				if (this.getAudioStatus) {
					this.$refs.burpAudio.play();
				}
			},
			modeValue(array) {
				if (array.length == 0) return null;
				var modeMap = {};
				var maxEl = array[0],
					maxCount = 1;
				for (var i = 0; i < array.length; i++) {
					var el = array[i];
					if (modeMap[el] == null) modeMap[el] = 1;
					else modeMap[el]++;
					if (modeMap[el] > maxCount) {
						maxEl = el;
						maxCount = modeMap[el];
					}
				}
				return maxEl;
			},
			getIngredientValue(value, ingredient) {
				// get the array of ingredient values
				const valueArr = this.allIngredients[value];
				// the most occured value in the array
				const indexValue = this.modeValue(valueArr);
				return ingredient[indexValue];
			},
			restartQuiz() {
				// Scroll to top of page when quiz is restarted
				window.scrollTo({
					top: 0,
					behavior: "smooth"
				});
				// Clear ingredient arrays
				this.$store.commit("clearIngredients");
				// Set global quiz started to false
				this.$store.commit("setQuizStarted", {
					value: false
				});
				// sets the isAnswered to true if answer is selected
				this.allQuestions.map(question => {
					this.$store.commit("setQuestionsToAnswered", {
						selected: question,
						value: false
					});
				});

				// reset all question isAnswered
				this.$store.commit("resetAllAnsweredStatus");

				// reset quiz status to false
				this.$store.commit("setQuizStatus", {
					result: false
				});
			}
		}
	};
</script>

<style lang="scss" scoped>
	.results-container {
		display: flex;
		flex-direction: column;
		position: relative;
	}

	#info-container {
		position: relative;
		z-index: 900;
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: center;
		margin: 0 auto;
		width: 80%;
		padding: 40px 0;
	}

	.ramen-info {
		margin-top: -40px;
	}

	strong {
		font-weight: 900;
	}

	.ramen-title {
		margin-top: 30px;
		font-size: 1.5rem;
		text-transform: lowercase;
	}

	.description-container p,
	.share-cta {
		font-size: 1rem;
		margin-bottom: 1.2rem;
		line-height: 1.4;
	}

	.quote {
		font-size: 1.2rem;
		font-weight: 700;
		font-style: italic;
	}

	@media (min-width: $breakpoint-small) {
		.ramen-title {
			font-size: 1.8rem;
		}

		.ramen-info p,
		.share-cta {
			font-size: 1.325em;
			line-height: 1.65em;
		}

		.quote {
			font-size: 1.4rem;
		}
	}

	@media (min-width: $breakpoint-medium) {
		#info-container {
			width: 60%;
			padding: 50px;
			overflow-y: auto;
		}

		.description-container p,
		.share-cta {
			font-size: 1.3rem;
		}

		.results-container {
			height: 100vh;
			overflow: hidden;
			width: 100%;
			display: flex;
			flex-direction: row-reverse;
		}

		.ramen-title {
			font-size: 2.5rem;
		}

		.ramen-info {
			padding: 0;
		}
	}
</style>
