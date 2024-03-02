<script setup lang="ts">
import { ref } from "vue"

const { toggleModal, showModal, addNote } = defineProps<{
  toggleModal: () => void
  addNote: (text: string) => void
  showModal: boolean
}>()

const handleClick = (e: MouseEvent) => {
  if (e.target === e.currentTarget) toggleModal()
}

const note = ref<string>("")

const validationText = ref<string>("")

const formSumbit = () => {
  if (!note.value.trim()) {
    validationText.value = "The field can't be empty"
    return
  }

  addNote(note.value.trim())
  note.value = ""
  validationText.value = ""
  toggleModal()
}
</script>
<template>
  <div class="fallback" :class="{ open: showModal }" @click="handleClick">
    <form @submit.prevent="formSumbit" class="addNoteForm">
      <textarea v-model="note" class="textField"></textarea>
      <p class="validationText" v-if="validationText">{{ validationText }}</p>
      <button class="addBtn">Add</button>
    </form>
  </div>
</template>

<style lang="scss" scoped>
.fallback {
  position: fixed;
  top: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  padding: 0 20px;
  background-color: rgba(0, 0, 0, 0.327);
  transition: var(--default-transition);
  visibility: hidden;
  opacity: 0;

  &::after {
    position: absolute;
    top: 20px;
    right: 20px;
    content: "x";
    font-size: 30px;
    line-height: 10px;
  }
}
.open {
  visibility: visible;
  opacity: 1;
}
.addNoteForm {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: start;
  width: 600px;
  height: 350px;
  border-radius: 20px;
  background-color: white;
  padding: 40px;

  @media (min-width: 768px) {
    height: 400px;
  }
}
.textField {
  width: 100%;
  height: calc(100% - 65px);
  border-radius: 10px;
  resize: none;
  padding: 20px;
}
.validationText {
  color: red;
}
.addBtn {
  background-color: orange;
  padding: 10px 20px;
  border-radius: 20px;

  &:hover,
  &:focus {
    background-color: rgb(205, 133, 0);
    transition: var(--default-transition);
  }
}
</style>
