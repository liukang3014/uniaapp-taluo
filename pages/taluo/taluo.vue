<template>
	<div>
		<!-- 卡片容器 -->
		<div class="cardsContainer" v-once>
			<div v-for="(card, index) in cards" :key="index" class="card" :style="calculatePosition(index)"
				@click="moveCard(index)" v-once>
				<transition name="flip" v-once>
					<div class="card-inner" :class="{ flipped: card.selected }" v-once>
						<div class="card-front custom-front"
							:style="{ backgroundColor: card.selected ? 'blue' : 'red' }">
							<p>{{ card.number }}</p>
						</div>
					</div>
				</transition>
			</div>
		</div>
		<div class="margins" v-once>
			<div class="div1"></div>
			<div class="div2"></div>
			<div class="div3"></div>
			<div class="div4"></div>
			<div class="div5"></div>
			<div class="div6"></div>

		</div>
		<div class="Popups">
			<div v-for="(item, index) in text" :key="index">{{ item }}</div>
		</div>

	</div>

</template>

<script>
	import {
		Textdata
	} from "@/taluo.js"
	import Eject from "@/components/Eject/Eject.vue"
	export default {
		components: {
			'Eject': Eject,
		},
		data() {
			return {
				cards: Array(54)
					.fill(null)
					.map((_, index) => ({
						number: index + 1,
						selected: false
					})),
				selectedCount: 0,
				DataText: false,
				text: [],
			};
		},
		methods: {

			gototext() {
				uni.navigateTo({
					url: "/pages/Text/Text"
				})
			},
			moveCard(index) {
				console.log(this.DataText);
				console.log(this.selectedCount);
				if (this.selectedCount >= 6) return;

				// 翻转卡片效果
				this.cards[index].selected = !this.cards[index].selected;
				if (this.selectedCount < 6) {

					const randomIndex = Math.floor(Math.random() * Textdata.length);
					const randomValue = Textdata[randomIndex];

					this.text.push(randomValue);
					console.log(this.text);

					const targetDiv = this.$el.querySelector(`.div${this.selectedCount + 1}`);
					const card = this.$el.querySelectorAll('.card')[index];
					// 计算目标位置的 top 和 left 坐标
					const targetTop = targetDiv.getBoundingClientRect().top + "px";
					const targetLeft = targetDiv.getBoundingClientRect().left + "px";
					card.style.top = targetTop;
					card.style.left = targetLeft;
					card.style.transform = 'rotateY(180deg)'; 
					this.selectedCount++;
					if (this.selectedCount == 6) {
						console.log(Textdata);
						setTimeout(() => {
							uni.navigateTo({
								url: "/pages/Text/Text"
							})
						}, 1000)
					}
				} else {
					return;
				}
			},
			calculatePosition(index) {
				const angle = (index - 27) * 1.5;
				const leftOffset = Math.sin(angle * (Math.PI / -200)) * 300;
				return {
					transform: `translate(-50%, -50%) rotate(${angle}deg) translateX(${leftOffset}px)`
				};
			},
			englishNumber(num) {
				const ones = ['One', 'Two', 'Three', 'Four', 'Five', 'Six', 'Seven', 'Eight', 'Nine'];
				const teens = ['Eleven', 'Twelve', 'Thirteen', 'Fourteen', 'Fifteen', 'Sixteen', 'Seventeen', 'Eighteen',
					'Nineteen'
				];
				const tens = ['Ten', 'Twenty', 'Thirty', 'Forty', 'Fifty'];
				if (num <= 9) return ones[num - 1];
				if (num <= 19) return teens[num - 11];
				if (num <= 99) {
					let t = Math.floor(num / 10);
					let o = num % 10;
					return tens[t - 1] + (o ? ' ' + ones[o - 1] : '');
				}
				return num;
			},

		},
	};
</script>

<style>
	.Popups {
		width: 150px;
		height: 200px;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}

	.cardsContainer {
		width: 100%;
		height: 400rpx;
	}

	.container {
		width: 100vw;
		height: 100vh;
	}

	.card-inner.flipped {
		transform: rotateY(180deg);
	}

	.card {
		width: 80px;
		height: 120px;
		border: 1px solid #000;
		border-radius: 10px;
		display: flex;
		justify-content: center;
		align-items: center;
		position: absolute;
		top: 10%;
		left: 50%;
		transform-origin: center;
		transition: top 0.5s, left 0.5s, transform 0.5s;
	}

	.card.flipping {
		animation: flipAndMove 0.5s forwards;
	}

	.margins {
		width: 100%;
		height: 1000rpx;
		position: relative;
	}

	.margins>div {
		width: 80px;
		height: 120px;
	}

	.margins>.div1 {
		position: absolute;
		top: 0px;
		left: 0px;
	}

	.margins>.div2 {
		position: absolute;
		top: 0px;
		right: 0px;
	}

	.margins>.div3 {
		position: absolute;
		top: 150px;
		left: 0px;
	}

	.margins>.div4 {
		position: absolute;
		top: 150px;
		right: 0px;
	}

	.margins>.div5 {
		position: absolute;
		top: 300px;
		left: 0px;
	}

	.margins>.div6 {
		position: absolute;
		top: 300px;
		right: 0px;
	}


	.custom-front {
		width: 100%;
		height: 100%;
		background-color: red;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.custom-back {
		background-color: blue;
		width: 100%;
		height: 100%;
		display: none;
	}

	.card-inner.flipped .custom-back {
		display: block;
	}
</style>