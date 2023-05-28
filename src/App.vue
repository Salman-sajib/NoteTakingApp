<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);

function getRandomColor() {
  return "hsl(" + Math.floor(Math.random() * 360) + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = "Note needs to be 10 characters or more");
  }

  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });

  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
};

const closeModal = () => {
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
          placeholder="Type here"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="closeModal">Close</button>
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
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Lora:wght@400;500;600;700&display=swap");

button,
textarea {
  font-family: inherit;
}

main {
  font-family: "Lora", serif;
  width: 100vw;
  min-height: 100dvh;
}

.container {
  width: min(100% - 1.5rem, 1200px);
  margin-inline: auto;
  padding-block: 1rem;
}

header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-block-end: 1.5rem;
}

h1 {
  color: rgb(25, 20, 20);
  font-weight: 600;
  font-size: 3.5rem;
}

header button {
  background-color: rgb(25, 20, 20);
  color: white;
  cursor: pointer;
  font-size: 1.7rem;
  width: 50px;
  height: 50px;
  border: none;
  border-radius: 50%;
}

.cards-container {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
}

@media (max-width: 520px) {
  .cards-container {
    justify-content: center;
  }
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  color: rgb(25, 20, 20);
  font-size: 0.9rem;
  padding: 0.8rem;
  border-radius: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 0.7rem;
  font-weight: 700;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgb(0, 0, 0, 0.77);
  z-index: 10;

  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  position: relative;
  width: min(100% - 2rem, 700px);
  background-color: white;
  display: flex;
  flex-direction: column;
  padding: 1.5rem;
  border-radius: 1rem;
}

textarea {
  font-size: 1rem;
  resize: none;
  border-radius: 0.5rem;
  padding: 10px;
}

.modal button {
  cursor: pointer;
  border: none;
  border-radius: 500px;
  padding: 0.7rem 1rem;
  font-size: 1rem;
  margin-top: 1rem;
  background-color: blueviolet;
  color: white;
  font-weight: 600;
}

.modal .close {
  cursor: pointer;
  background-color: rgb(189, 15, 15);
  margin-top: 0.5rem;
}

.modal p {
  color: rgb(189, 15, 15);
}
</style>
