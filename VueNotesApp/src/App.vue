<script setup>
import { ref } from "vue";
const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errorMessage = ref("");

function getRandomColor() {
	return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
	if (newNote.value.length <= 9) {
		errorMessage.value = "The note has to be 10 characters or more.";
		return;
	}
	notes.value.push({
		id: Math.floor(Math.random() * 10000),
		text: newNote.value,
		date: new Date(),
		backgroundColor: getRandomColor(),
	});
	showModal.value = false;
	newNote.value = "";
	errorMessage.value = "";
};
</script>

<template>
	<main>
		<div v-if="showModal" class="overlay">
			<div class="modal">
				<textarea
					v-model.trim="newNote"
					name="note"
					id="note"
					cols="30"
					rows="10"
				></textarea>
				<p v-if="errorMessage" style="color: red">{{ errorMessage }}</p>
				<button @click="addNote">Add Note</button>
				<button @click="showModal = false" class="close">close</button>
			</div>
		</div>

		<div class="container">
			<header>
				<h1>Notes</h1>
				<button @click="showModal = true">+</button>
			</header>
			<div class="cards-container">
				<div
					v-for="note in notes"
					:key="note.id"
					class="card"
					:style="{ backgroundColor: note.backgroundColor }"
				>
					<p class="main-text">{{ note.text }}</p>
					<p class="date">{{ note.date.toISOString().split("T")[0] }}</p>
				</div>
			</div>
		</div>
	</main>
</template>

<style scoped>
main {
	height: 100vh;
	width: 100vw;
}
.container {
	max-width: 1000px;
	padding: 10px;
	margin: 0 auto;
}

header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	border-bottom: 2px solid black;
	border-color: black;
}

h1 {
	font-weight: bold;
	margin-bottom: 25px;
	font-size: 75px;
	color: black;
}

header button {
	border: none;
	padding: 10px;
	width: 50px;
	height: 50px;
	cursor: pointer;
	background-color: black;
	border-radius: 100%;
	color: white;
	font-size: 20px;
}

.cards-container {
	display: flex;
	flex-wrap: wrap;
}

.card {
	width: 225px;
	height: 225px;
	margin: 10px;
	padding: 20px;
	border-radius: 15px;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
}

.overlay {
	position: absolute;
	width: 100%;
	height: 100%;
	background-color: rgba(0, 0, 0, 0.77);
	z-index: 10;
	display: flex;
	align-items: center;
	justify-content: center;
}

.modal {
	width: 750px;
	background-color: white;
	border-radius: 10px;
	padding: 30px;
	position: relative;
	display: flex;
	flex-direction: column;
}

.modal button {
	padding: 10px 20px;
	font-size: 20px;
	width: 100%;
	background-color: blueviolet;
	border: none;
	color: white;
	cursor: pointer;
	margin-top: 15px;
}

.modal .close {
	margin-top: 5px;
	background-color: red;
}
</style>
