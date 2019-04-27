<template>
  <div class="container">
    <div class="tag-serch" v-show="isSerchingByTag"></div>
    <div class="note-editor" v-show="isntSearchingByTag">
      <textarea v-model="text" placeholder="Enter your note here..." rows="5"></textarea>
      <div class="button-container">
        <button
          class="add-button"
          @click="submitNote"
          :disabled="isSubmitDisabled"
        >{{ editNoteItem ? 'Save changes' : 'Add note' }}</button>
        <button v-show="editNoteItem" @click="cancelEditNote" class="cancel-button">Cancel</button>
        <color-picker v-show="isntEditing" @color-changed="colorChangedHandler"/>
      </div>
    </div>
  </div>
</template>

<script>
import ColorPicker from "@/components/ColorPicker.vue";

export default {
  name: "NotesEditor",

  data: () => ({
    text: "",
    editNoteItem: null,
    bgColor: "#000000",
    tag: null
  }),

  components: {
    ColorPicker
  },

  methods: {
    submitNote() {
      if (this.editNoteItem) {
        this.editNote();
      } else {
        this.addNote();
      }
    },
    addNote() {
      const note = {
        id: new Date().getTime(),
        text: this.text,
        bgcolor: this.bgColor
      };

      this.$emit("addNote", note);
      this.text = "";
    },
    setEditNote(note) {
      if (this.editNoteItem) {
        return;
      }

      this.editNoteItem = note;
      this.text = note.text;
    },
    editNote() {
      const note = {
        ...this.editNoteItem,
        text: this.text
      };
      this.$emit("saveChangesNote", note);
      this.editNoteItem = null;
      this.text = "";
    },
    cancelEditNote() {
      this.editNoteItem = null;
      this.text = "";
    },
    colorChangedHandler(color) {
      this.bgColor = color;
    }
  },
  computed: {
    isSubmitDisabled() {
      return this.text.trim().length === 0;
    },
    isSerchingByTag() {
      return !!this.tag;
    },
    isntSearchingByTag() {
      return !this.tag;
    },
    isntEditing() {
      return !this.editNoteItem;
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
  max-width: 600px;
}

.note-editor {
  width: 100%;
  max-width: 600px;
  padding: 16px;
  margin: 16px auto;
  background-color: white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  border-radius: 2px;
  display: flex;
  flex-direction: column;
}

textarea {
  width: 100%;
  resize: none;
  margin: 5px;
  font-size: 14px;
  border: none;
  font-weight: 300;
}

textarea:focus {
  outline: 0;
}

.button-container {
  display: flex;
  flex-flow: row-reverse nowrap;
  align-items: center;
}

%button {
  width: 100px;
  border-radius: 8px;
  cursor: pointer;
  color: #ffffff;
  font-size: 14px;
  padding: 8px 8px;
  text-transform: uppercase;
  text-decoration: none;
  text-shadow: 0px 1px 0px #2f6627;
}

%button:active {
  position: relative;
  top: 1px;
}

%button:focus {
  outline: 0;
}

.add-button {
  @extend %button;
  background-color: #44c767;
  border: 1px solid #18ab29;
  width: 130px;
}

.add-button:hover {
  background-color: #5cbf2a;
}

.cancel-button {
  @extend %button;
  background-color: #c74444;
  border: 1px solid #ab1818;
  margin-right: 20px;
}

.cancle-button:hover {
  background-color: #bf2a2a;
}

.color-button {
  @extend %button;
  background-color: #44c767;
  border: 1px solid #18ab29;
  margin-right: auto;
  width: 150px;
}

.color-button:hover {
  background-color: #5cbf2a;
}
</style>
