<template>
  <div class="notes-grid" ref="grid">
    <NoteItem
      v-for="note in notes"
      :key="note.messageId"
      :note="note"
      @removeNote="removeNoteHandler"
      @editNote="editNoteHandler"
    />
  </div>
</template>

<script>
import Masonry from "masonry-layout";
import NoteItem from "@/components/NoteItem.vue";

export default {
  name: "notes-grid",
  props: {
    notes: {
      type: Array,
      required: true
    }
  },

  components: {
    NoteItem
  },

  methods: {
    removeNoteHandler(noteId) {
      this.$emit("removeNote", noteId);
    },
    editNoteHandler(note) {
      this.$emit("editNote", note);
    }
  },

  mounted() {
    const grid = this.$refs.grid;
    this.msnry = new Masonry(grid, {
      itemSelector: ".note",
      columnWidth: 200,
      gutter: 10,
      isFitWidth: true
    });
  },

  updated() {
    this.msnry.reloadItems();
    this.msnry.layout();
  }
};
</script>

<style lang="scss" scoped>
.notes-grid {
  margin: 0 auto;
}
</style>
