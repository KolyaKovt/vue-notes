<script setup lang="ts">
import { ref } from "vue"
import { nanoid } from "nanoid"
import { format } from "date-fns"
import { Note } from "./types/index"
import CardsList from "./components/CardsList.vue"
import Modal from "./components/Modal.vue"

const getStoredNotes = () => {
  const storedNotes = localStorage.getItem("notes")
  return storedNotes ? JSON.parse(storedNotes) : []
}

const setStoredNotes = () => {
  localStorage.setItem("notes", JSON.stringify(notes.value))
}

const notes = ref<Note[]>(getStoredNotes())

const showModal = ref<boolean>(false)

const deleteNote = (id: string) => {
  notes.value = notes.value.filter(note => note.id !== id)

  setStoredNotes()
}

const toggleModal = () => {
  showModal.value = !showModal.value
}

const onSubmit = (note: string) => {
  notes.value.push({
    id: nanoid(),
    text: note,
    date: format(new Date(), "dd.MM.yyyy"),
  })

  setStoredNotes()
}
</script>

<template>
  <main>
    <section class="container">
      <div class="mainTitleContainer">
        <h1 class="mainTitle">Notes</h1>

        <button class="addBtn" type="button" @click="toggleModal">+</button>
      </div>

      <CardsList :notes="notes" :deleteNote="deleteNote" />
    </section>
  </main>

  <Modal
    :onSubmit="onSubmit"
    :showModal="showModal"
    :toggleModal="toggleModal"
  />
</template>

<style lang="scss" scoped>
.container {
  padding: 50px;
  max-width: 1000px;
  margin: 0 auto;
}
.mainTitleContainer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 30px;
  user-select: none;
}
.mainTitle {
  font-size: 50px;

  @media (min-width: 768px) {
    font-size: 60px;
  }
}
.addBtn {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: black;
  color: white;
  font-size: 20px;
}
</style>
