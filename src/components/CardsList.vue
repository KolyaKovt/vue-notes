<script setup lang="ts">
import { Note } from "../types"

const { notes } = defineProps<{
  notes: Note[]
  deleteNote: (id: string) => void
}>()
</script>

<template>
  <ul class="list">
    <li class="card" v-for="note in notes" :key="note.id">
      <button
        class="deleteBtn"
        type="button"
        @click="() => deleteNote(note.id)"
      >
        x
      </button>
      <p class="text">{{ note.text }}</p>
      <p>{{ note.date }}</p>
    </li>
  </ul>
</template>

<style lang="scss" scoped>
.list {
  display: grid;
  grid-template-columns: 1fr;
  align-items: start;
  gap: 20px;
  padding: 0;
  margin: 0;
  list-style: none;

  @media (min-width: 768px) {
    grid-template-columns: 1fr 1fr;
  }
  @media (min-width: 1000px) {
    grid-template-columns: 1fr 1fr 1fr;
  }
}
.card {
  position: relative;
  background-color: orange;
  padding: 10px;
  border-radius: 10px;
  padding: 25px 10px 10px 10px;
  transition: var(--default-transition);

  &:hover,
  &:focus {
    background-color: rgb(205, 133, 0);
  }
}
.deleteBtn {
  position: absolute;
  top: 15px;
  right: 15px;
  line-height: 0;
  background-color: inherit;
}
.text {
  margin-bottom: 40px;
}
</style>
