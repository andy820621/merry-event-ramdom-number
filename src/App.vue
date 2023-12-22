<template>
	<div class="main-content">
		<div class="img-box">
			<img class="main-img" src="./assets/background-number.png" alt="" />
			<div class="circle">
				<div class="result">{{ selectedNumber }}</div>
			</div>

			<div class="btn" @click.prevent="generateRandomNumbers">
				<img src="./assets/tree.png" alt="" />
			</div>
			<div class="inputBox">
				<label for="count">需要产出的数字个数: </label>
				<select id="count" v-model="count">
					<option value="2">2</option>
					<option value="3">3</option>
					<option value="4">4</option>
					<option value="8">8</option>
					<option value="25">25</option>
				</select>
			</div>
		</div>
	</div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, computed } from "vue";
const count = ref(2);
function randomFromOneToCount(count: number) {
	return Math.floor(Math.random() * count) + 1;
}
const selectedNumber = ref();
const selectedNumbers = ref<number[]>([]);
function generateRandomNumbers() {
	if (selectedNumbers.value.length >= count.value) {
		alert("所有數字已經抽選完畢！");
		selectedNumbers.value = [];
		selectedNumber.value = undefined;
		return;
	}

	let num;
	do {
		num = randomFromOneToCount(count.value);
	} while (selectedNumbers.value.includes(num));
	selectedNumber.value = num;
	selectedNumbers.value.push(num);
}
watch(selectedNumbers, () => {
	console.log("now selectedNumbers", selectedNumbers.value);
});
watch(count, () => {
	selectedNumbers.value = [];
	selectedNumber.value = undefined;
});

function confirmRestart() {
	const shouldRestart = window.confirm("是否要重新開始？");
	if (shouldRestart) {
		localStorage.removeItem("selectedNumbersArray");
		window.location.reload();
	}
}
</script>

<style scoped lang="scss">
.main-content {
	width: 100%;
	height: 100vh;
	display: flex;
	justify-content: center;
	.img-box {
		position: relative;
		.current-number,
		.circle,
		.btn,
		.inputBox,
		.restart {
			position: absolute;
		}
		.restart {
			bottom: 1rem;
			left: 50%;
			transform: translateX(-50%);
			z-index: 3;
			// 按鈕樣式
			background-color: #ff6600;
			color: #fff;
			padding: 10px 20px;
			border: none;
			border-radius: 5px;
			cursor: pointer;
			transition: background-color 0.3s ease;

			&:hover {
				background-color: #ff8800;
			}

			&:active {
				background-color: #ff4400;
				transform: translateX(-50%) scale(0.95);
			}
		}
		.inputBox {
			width: 9.5%;
			aspect-ratio: 1;
			border-radius: 50%;
			left: 24.5%;
			top: 35%;
			z-index: 4;
			display: grid;
			label {
				display: none;
			}
			select {
				-webkit-appearance: none;
				-moz-appearance: none;
				appearance: none;
				background-color: transparent;
				border: none;
				padding: 10px;
				margin: 0;
				width: 100%;
				font-family: inherit;
				font-size: inherit;
				cursor: pointer;
				font-size: 2.4vw;
				text-align: center;
			}
		}
		.btn {
			width: 7.5%;
			aspect-ratio: 10 / 14;
			top: 15%;
			right: 2%;
			z-index: 3;
			cursor: pointer;
			&:active {
				transform: scale(0.95);
			}
			img {
				width: 100%;
				object-fit: cover;
				object-position: center;
			}
		}
		.current-number {
			width: 20.2%;
			height: 25.6%;
			top: 9.88%;
			right: 1.9%;
			z-index: 3;
			display: grid;
			place-items: center;
		}
		.circle {
			width: 19%;
			aspect-ratio: 1;
			top: 43%;
			left: 36.5%;
			z-index: 2;
			border-radius: 50%;

			.result {
				display: grid;
				place-items: center;
				width: 100%;
				height: 100%;
				font-size: 5vw;
			}
		}
		.main-img {
			position: relative;
			height: 100%;
			object-fit: cover;
			object-position: center;
			z-index: 2;
			pointer-events: none;
		}
	}
}

ul {
	list-style-type: none;
	padding: 0;
}

li {
	margin: 5px 0;
	font-size: 1.5rem;
}

.numberArray {
	display: flex;
	gap: 0.24rem;
	margin-bottom: 1rem;
	li {
		width: 50px;
		height: 50px;
		font-size: 1.5rem;
		font-weight: 800;
		border: 1px solid #000;
		border-radius: 50%;
		display: grid;
		place-items: center;
	}
	&.bigText {
		margin-bottom: 0;
		flex-wrap: wrap;
		width: 100%;
		height: 100%;
		justify-content: center;
		align-items: center;
		column-gap: 0.8vw;
		row-gap: 0.01vw;
		li {
			--num: 5vw;
			width: calc(var(--num) * 3 / 2);
			height: calc(var(--num) * 3 / 2);
			font-size: var(--num);
		}
	}
}
ul,
li {
	padding: 0;
	margin: 0;
}
</style>
