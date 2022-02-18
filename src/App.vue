
<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-6 text-center mt-5">
        <Create />
      </div>
    </div>
    <div class="row">
      <div class="col-md-6">
        <form class="bg-warning p-4 mb-4 rounded" @submit.prevent="">
          <div class="form-group">
            <label for="title" class="form-label">Title</label>
            <input
              type="text"
              id="title"
              v-model="title"
              class="form-control"
            />
          </div>
          <div class="form-group">
            <label for="note" class="form-label">Note</label>
            <input type="text" id="note" v-model="note" class="form-control" />
          </div>
          <div v-if="edit == false">
            <button
              class="btn btn-primary mt-3"
              type="submit"
              v-on:keyup.enter="addNote"
              @click="addNote()"
            >
              create
            </button>
          </div>
          <div v-if="edit == true">
            <button class="btn btn-primary mt-3" @click="updateNote()">
              Update
            </button>
            <button class="btn btn-danger mt-3 ms-3" @click="cancelNote()">
              Cancel
            </button>
          </div>
        </form>
      </div>
      <div class="col-md-6">
        <table class="table table-striped table-success table-hover">
          <thead>
            <tr>
              <th>No</th>
              <th>Title</th>
              <th>Notes</th>
              <th>Aksi</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(note, index) in notes" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ note.title }}</td>
              <td>{{ note.note }}</td>

              <td>
                <button
                  @click="editNote(note, index)"
                  class="btn btn-warning btn-sm"
                >
                  Edit
                </button>
                <button
                  @click="DeleteNoteItem(index) & confim('iyakah?')"
                  class="btn btn-danger ms-2 btn-sm"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { walkBlockDeclarations } from "@vue/compiler-core";
import { onMounted, ref } from "vue";
import Create from "./components/Create.vue";

const note = ref("");
const title = ref("");
const notes = ref([]);
const number = ref(1);
const edit = ref(false);
const ind = ref(0);

function addNote() {
  const newNote = {
    id: Math.floor(Math.random() * 100),
    title: title.value,
    note: note.value,
  };
  notes.value.push(newNote);
  localStorage.notes = JSON.stringify(notes.value);
  title.value = "";
  note.value = "";
}

onMounted(() => {
  const noteData = localStorage.notes;
  if (noteData) {
    notes.value = JSON.parse(noteData);
  }
});
function editNote(t, i) {
  edit.value = !edit.value;
  title.value = t.title;
  note.value = t.note;
  ind.value = i;
}
function updateNote() {
  edit.value = !edit.value;
  const notedb = {
    id: Math.floor(Math.random() * 100),
    title: title.value,
    note: note.value,
  };
  notes.value[ind.value] = notedb;
  localStorage.notes = JSON.stringify(notes.value);
  let noteData1 = localStorage.notes;
  let noteDB = JSON.parse(noteData1);
  notes.value = noteDB;
  title.value = "";
  note.value = "";
}
function cancelNote() {
  title.value = "";
  note.value = "";
  edit.value = !edit.value;
}
function DeleteNoteItem(index) {
  confirm("Benar Mau hapus ?");
  if (confirm) {
    // notes.value = notes.value.filter((note) => note.id !== id);
    notes.value.splice(index, 1);
    localStorage.notes = JSON.stringify(notes.value);
  } else {
    alert("Hampir saja");
  }
}
</script>
<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
