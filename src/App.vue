<script setup>
import { ref } from 'vue'

const showModal = ref(false)
const newNote = ref('')
const notes = ref([])
const id = ref(1)
const errorMessage = ref('')

const color = () => {
  return 'hsl(' + Math.random() * 360 + ', 100%, 75%)'
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = 'Note needs to have 10 or more characters!')
  }

  notes.value.push({
    id: id.value++,
    text: newNote.value,
    date: new Date(),
    backgroundColor: color()
  })
  newNote.value = ''
  showModal.value = false
  errorMessage.value = ''
}
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
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
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
          :style="{ backgroundColor: note.backgroundColor }"
          class="card"
        >
          <p class="main-text">
            {{ note.text }}
          </p>
          <div class="currentDate">
            <p class="date">{{ note.date.toLocaleDateString('en-Us') }}</p>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

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
  font-family: 'Lato', sans-serif;
  font-weight: bold;
  font-size: 75px;
  color: rgb(43, 41, 41);
  margin-bottom: 25px;
}

.modal p {
  color: red;
  margin-top: 2px;
}

h1::before {
  content: '';
  position: absolute;
  background-color: rgba(8, 192, 177, 0.715);
  top: 100px;
  height: 5px;
  width: 400px;
  min-width: 100px;
}

header button {
  border: 0;
  padding: 10px;
  border-radius: 100%;
  background-color: rgba(0, 0, 0, 0.872);

  color: #fff;
  font-size: 20px;
  width: 50px;
  height: 50px;
  cursor: pointer;
}

header button:hover {
  background-color: rgba(0, 0, 0, 0.968);
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.card {
  width: 225px;
  height: 225px;
  border-radius: 15px;
  padding: 10px;
  margin: 0 20px 20px 0;
  background-color: rgb(220, 150, 110);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  box-shadow: -2px 2px 2px 1px rgb(150, 143, 143);
  font-family: 'Lato', sans-serif;
}

.main-text {
  color: rgb(37, 35, 35);
}

.date {
  font-size: 12.5px;
  font-weight: bold;
  font-style: italic;
}

.currentDate {
  display: flex;
  justify-content: space-between;
}

.time {
  font-style: italic;
  font-size: 12.5px;
}

textarea {
  outline: none;
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
  background-color: #fff;
  border-radius: 10px;
  position: relative;
  display: flex;
  flex-direction: column;
  padding: 20px;
}

.modal button {
  padding: 10px 20px;
  width: 100%;
  background-color: olivedrab;
  cursor: pointer;
  border: none;
  margin-top: 15px;
  font-size: 20px;
  color: white;
  text-transform: uppercase;
  font-family: 'Lato', sans-serif;
  border-radius: 5px;
}

.modal .close {
  background-color: rgb(190, 69, 17);
  color: white;
  margin-top: 6px;
}
</style>
