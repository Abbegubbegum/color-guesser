<template>
	<main>
		<div
			class="color-box"
			:style="{ backgroundColor: colors[colorIndex] }"
		></div>
		<div class="btn-group">
			<button type="button" @click="buttonClicked(0)">
				{{ colors[0] }}
			</button>
			<button type="button" @click="buttonClicked(1)">
				{{ colors[1] }}
			</button>
			<button type="button" @click="buttonClicked(2)">
				{{ colors[2] }}
			</button>
		</div>
		<p class="popup correct" :class="{ show: answered && correct }">
			Correct!
		</p>
		<p class="popup incorrect" :class="{ show: answered && !correct }">
			Incorrect! Pick again
		</p>
	</main>
</template>

<script setup lang="ts">
import { ref } from "vue";

let colors = ref([] as string[]);

let colorIndex = ref(0);
randomizeColors();

let answered = ref(false);
let correct = ref(false);

function randomizeColors() {
	colors.value = [
		createRandomColor(),
		createRandomColor(),
		createRandomColor(),
	];

	colorIndex.value = Math.floor(Math.random() * colors.value.length);
}

function createRandomColor(): string {
	//Create a random hexadecimal color
	let r = Math.floor(Math.random() * 255).toString(16);
	let g = Math.floor(Math.random() * 255).toString(16);
	let b = Math.floor(Math.random() * 255).toString(16);

	r = r.length === 1 ? "0" + r : r;
	g = g.length === 1 ? "0" + g : g;
	b = b.length === 1 ? "0" + b : b;

	return `#${r + g + b}`;
}

function buttonClicked(index: number): void {
	answered.value = true;
	correct.value = index === colorIndex.value;

	if (correct.value) {
		randomizeColors();
	}
}
</script>

<style scoped>
main {
	height: 100%;
	width: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}
.color-box {
	width: 30vw;
	height: 30vh;
	background-color: white;
}

.btn-group {
	margin: 2rem;
	width: 30vw;
	display: flex;
	justify-content: space-around;
	align-items: center;
}

button {
	padding: 0.5rem;
	color: black;
	text-align: center;
	font-size: 1.2rem;
	font-weight: bold;
}

.popup {
	display: none;
	font-size: 2rem;
	font-weight: bolder;
}

.correct {
	color: lightgreen;
}

.incorrect {
	color: red;
}

.show {
	display: inline;
}
</style>
