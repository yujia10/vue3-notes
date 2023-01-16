
<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errorMsg = ref("");

function getRandomColor() {
  let color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  return color;
}

const addNote = () => {
  // validation
  if (newNote.value.length < 10) {
    return (errorMsg.value = "Note needs to be at least 10 characters.");
  }
  // push a new note to notes array
  notes.value.push({
    id: Math.floor(Math.random() * 100000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });
  // close the modal and empty content
  showModal.value = false;
  newNote.value = "";
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
        >
        </textarea>
        <p v-if="errorMsg">{{ errorMsg }}</p>
        <button @click="addNote()">Add Note</button>
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
          <p class="date">{{ note.date.toLocaleDateString("en-GB") }}</p>
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
}
h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 50px;
}
header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
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
  background-color: aliceblue;
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  color: black;
}
.date {
  font-size: 12px;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  width: 750px;
  background: white;
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
  background: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}
.modal .close {
  background: coral;
  margin-top: 15px;
}
.modal p {
  color: coral;
}
</style>
