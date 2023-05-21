<template>
	<div class="Content">
		<div class="Select-time">
			<div class="Slider">
				<input type="range" min="1" max="100" step="1" v-model="time" />
			</div>
			<input class="timeText" type="number" v-model="time" min="1" max="100" @input="checkTime" />
			<p class="tiny-text time">Время: {{ time }}с</p>
		</div>
		<div class="Click-actions">
			<div class="Click-button" :class="{ clicked: isClicked, 'disabled-button': isBlocked }" :disabled="isBlocked" @click="pressButton()">
				<p class="button-text">{{ clickPhrase }}</p>
			</div>
			<div class="footer-section">
				<div class="info">
					<p class="tiny-text">Кликов: {{ resultClicks }}</p>
					<p class="tiny-text">Осталось: {{ remainTime }}.{{ remainMilliseconds }}с</p>
				</div>
				<button class="Reset-button" @click="resetAll">Сброс</button>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "MainComponent",
	data() {
		return {
			time: 1,
			countClicks: 0,
			remainTime: 0,
			remainMilliseconds: 0,
			timer: null,
			resultClicks: 0,
			isClicked: false,
			isBlocked: false,
			clickPhrase: "Нажми на меня!",
		};
	},
	methods: {
		checkTime() {
			const maxValue = 100;
			if (this.time > maxValue) {
				this.time = maxValue;
			}
		},
		pressButton() {
			this.countClicks++;
			this.resultClicks = this.countClicks;
			this.isClicked = true;
			setTimeout(() => {
				this.isClicked = false;
			}, 200);
			if (this.timer) return;

			this.remainTime = this.time;
			this.remainMilliseconds = 0;
			this.timer = setInterval(() => {
				if (this.remainTime > 0 || this.remainMilliseconds > 0) {
					if (this.remainMilliseconds === 0) {
						this.remainMilliseconds = 9;
						this.remainTime--;
					} else {
						this.remainMilliseconds--;
					}
				} else {
					clearInterval(this.timer);
					this.countClicks = 0;
					this.isBlocked = true;
					this.timer = null;
				}
			}, 100);
		},
		resetAll() {
			this.time = 1;
			this.isClicked = false;
			this.countClicks = 0;
			this.resultClicks = 0;
			this.remainTime = 0;
			this.remainMilliseconds = 0;
			this.isBlocked = false;

			if (this.timer) {
				clearInterval(this.timer);
				this.timer = null;
			}
		},
	},
	updated() {
		if (this.countClicks > 0) {
			this.clickPhrase = "Кликай";
		} else {
			this.clickPhrase = "Нажми на меня!";
		}
		if (this.isBlocked) {
			setTimeout(() => {
				this.isBlocked = false;
			}, 1000);
		}
	},
	computed: {
		formattedMilliseconds() {
			return (this.remainMilliseconds * 100).toString();
			//* В коде используется вычисляемое свойство formattedMilliseconds,
			//* которое преобразует значение remainMilliseconds в формат с тремя цифрами,
			//* добавляя ведущие нули при необходимости.
		},
	},
};
</script>

<style scoped lang="scss">
@keyframes scaleUp {
	0% {
		transform: scale(1);
	}
	100% {
		transform: scale(1.1);
	}
}

.Content {
	touch-action: manipulation;
	width: 100%;
	height: 100%;
	font-size: 2rem;
	-webkit-touch-callout: none; /* iOS Safari */
	-webkit-user-select: none; /* Safari */
	-khtml-user-select: none; /* Konqueror HTML */
	-moz-user-select: none; /* Old versions of Firefox */
	-ms-user-select: none; /* Internet Explorer/Edge */
	user-select: none; /* Non-prefixed version, currently */

	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	.Select-time {
		width: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		.Slider {
			min-width: 20%;
			max-width: 50%;
			margin-bottom: 1rem;
			input {
				width: 100%;
			}
		}
		.timeText {
			border: 2px solid #8b8b8b;
			border-radius: 20px;
			padding: 1rem 0;
			text-align: center;
			font-size: 2.5rem;
			min-width: 10%;
			max-width: 20%;
		}
		.time {
			margin-top: 5rem;
		}
	}
	.Click-actions {
		touch-action: manipulation;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		.Click-button {
			touch-action: manipulation;
			display: flex;
			justify-content: center;
			align-items: center;
			background-color: #2b2b2b;
			width: 25rem;
			height: 25rem;
			border-radius: 100%;
			transform: scale(1);
			transition: all 0.1s ease;
			opacity: 1;
			font-size: 2.5rem;
		}
		.Click-button:hover {
			transform: scale(1.1);
			background: rgba(43, 43, 43, 0.88);
			cursor: pointer;
		}
		.disabled-button {
			opacity: 0.7; /* Уменьшение прозрачности */
			pointer-events: none; /* Изменение курсора */
			cursor: default;
		}
		.clicked {
			animation: scaleUp 0.1s;
		}
		.button-text {
			color: #ffff;
			-webkit-touch-callout: none; /* iOS Safari */
			-webkit-user-select: none; /* Safari */
			-khtml-user-select: none; /* Konqueror HTML */
			-moz-user-select: none; /* Old versions of Firefox */
			-ms-user-select: none; /* Internet Explorer/Edge */
			user-select: none; /* Non-prefixed version, currently */
		}
	}

	.footer-section {
		margin-top: 2rem;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		.info {
			display: flex;
			gap: 3rem;
		}
		.Reset-button {
			border: 2px solid #cc726d;
			color: #cc726d;
			border-radius: 40px;
			transition: all 0.2s ease;
			cursor: pointer;
			width: 25rem;
			height: 6rem;
			font-size: 2.5rem;
			font-weight: 100;
		}
		.Reset-button:hover {
			background: #cc726d;
			color: #fff;
		}
	}
}
</style>
