<template>
  <div id="app">
    <h3>ノート一覧</h3>
    <button id="addButton" v-on:click="addNote">追加</button>
    <tr v-for="(note, index) in notes" :key="index">
      <td>{{ note.noteName }}</td>
    </tr>
  </div>
</template>

<script>

import axios from "axios";

export default {
  data: function () {
    return {
      notes: []
    }
  },
  methods: {
    getNotes() {
      axios.get("http://127.0.0.1:8000/api/note")
          .then((res) => {
            this.notes = res.data;
          });
    },
    addNote: function () {
      axios.get("http://127.0.0.1:8000/api/note")
          .then((res) => {
            this.notes = res.data;
          });
    }
  },
  mounted() {
    this.getNotes();
  }
}
</script>

<style>
#app {
  height: 100vh;
}
button.transparent {
  margin: 5px;
  background: transparent;
  border: none;
}
input.transparent {
  width: 100%;
  border: none;
}
input.transparent:focus {
  outline: none;
  -webkit-box-shadow: none;
  box-shadow: none;
}
</style>