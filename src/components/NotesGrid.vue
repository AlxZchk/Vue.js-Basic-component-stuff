<template>
  <div class="container">
    <div class="tag-search" v-show="isSearchShown">Searching posts with <span class="hashtag">#{{searchTag}}</span> tag. <span @click="cancelTagSearch" class="cancel-tag-search">Cancel search</span></div>
    <div class="notes-grid" ref="grid">
      <NoteItem
        @tag-clicked="tagClickedHandler"
        v-for="note in listItems"
        :key="note.messageId"
        :note="note"
        @removeNote="removeNoteHandler"
        @editNote="editNoteHandler"
      />
    </div>
  </div>
</template>

<script>
import Masonry from "masonry-layout";
import NoteItem from "@/components/NoteItem.vue";

export default {
  name: "notes-grid",
  data: () => ({
    searchTag: null
  }),
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
    },
    tagClickedHandler(tag) {
      this.searchTag = tag;
    },
    cancelTagSearch() {
      this.searchTag = null;
    }
  },
  computed: {
    listItems() {
      const notes = this.notes;
      debugger;
      return notes.filter(note => {
        const noteText = note.text;
        const tags = [];
        noteText.split(" ").map(word => {
          if (word[0] === "#" && word.length > 1) {
            tags.push(word.substr(1));
          }
        });

        return this.searchTag
          ? !!tags.find(tag => tag === this.searchTag)
          : true;
      });
    },
    isSearchShown() {
      return !!this.searchTag;
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
.container {
  margin: 0 auto;
  width: 100%;
}

.notes-grid {
  margin: 0 auto;
}

.tag-search {
  margin: 0 auto;
  text-align: center;
  font-size: 18px;
  margin-bottom: 30px;
}

.cancel-tag-search {
  text-decoration: underline;
  color: red;
  cursor: pointer;
}
</style>
