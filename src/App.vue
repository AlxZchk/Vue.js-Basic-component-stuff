<template>
  <div class="notes-app">
    <notes-header :title="title"/>
    <notes-editor @addNote="addNoteHandler" @saveChangesNote="saveNote" ref="edit"/>
    <notes-grid :notes="notes" @removeNote="removeNoteHandler" @editNote="editNoteHandler"/>
  </div>
</template>

<script>
import VueColorPickerBoard from "vue-color-picker-board";
import NotesHeader from "@/components/NotesHeader.vue";
import NotesEditor from "@/components/NotesEditor.vue";
import NotesGrid from "@/components/NotesGrid.vue";

const notes = localStorage.notes ? JSON.parse(localStorage.notes) : [];

export default {
  name: "notes-app",
  components: {
    NotesHeader,
    NotesEditor,
    NotesGrid,
    VueColorPickerBoard
  },
  data: () => ({
    title: `Alexey's Imageboard`,
    notes
  }),

  methods: {
    addNoteHandler(note) {
      this.notes = [...this.notes, note];
      this.updateLocalStorage();
    },
    removeNoteHandler(noteId) {
      this.notes = this.notes.filter(({ id }) => id !== noteId);
      this.updateLocalStorage();
    },
    editNoteHandler(note) {
      this.$refs.edit.setEditNote(note);
    },
    saveNote(note) {
      this.notes = this.notes.map((item) => item.id === note.id ? note : item);
      this.updateLocalStorage();
    },
    updateLocalStorage() {
      localStorage.notes = JSON.stringify(this.notes);
    },
    colorSelectionHandler(color) {
      console.log(color);
    },
  }
};
</script>

<style lang="scss">
$color: #eaeaea;

* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  font-weight: 300;
  background-color: $color;
}

.notes-app {
  display: flex;
  width: 100%;
  max-width: 980px;
  flex-direction: column;
  align-items: center;
  margin: 0 auto;
}

.hashtag {
  color: lightblue;
  text-decoration: underline;
  cursor: pointer;
}
</style>
