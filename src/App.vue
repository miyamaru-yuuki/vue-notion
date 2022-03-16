<template>
  <div id="app">
    <h3>ノート一覧</h3>
    <form class="add-form" v-on:submit.prevent="addNote">
      ノート名 <input type="text" v-model="noteName">
      <button type="submit">追加</button>
    </form>
    <tr v-for="(note, index) in this.notes" :key="index">
      <td v-if="!note.isActive" key="input-email">{{ note.noteName }}</td>
      <td v-if="note.isActive" key="input-email">aaa</td>
      <td><button id="updButton" v-show="!note.isActive" v-on:click="updNote(note)">編集</button></td>
      <td><button id="delButton" v-on:click="delNote(note)">削除</button></td>
    </tr>
  </div>
</template>

<script>
import axios from "axios";

export default {
data: function () {
return {
  notes: [],
  noteName: ''
}
},
methods: {
  getNotes() {
    axios.get("http://127.0.0.1:8000/api/note")
        .then((res) => {
          this.notes = res.data;
          this.notes.forEach(function (note) {
            note.isActive = false
          });
          console.log(this.notes);
        });
  },
  addNote: function () {
    axios.post("http://127.0.0.1:8000/api/note", {
      noteName: this.noteName
    }).then(() => {
      //this.getNotes();
    });
  },
  updNote: function (note) {
    this.$set(note, "isActive", true);
    console.log(this.notes);
  },
  delNote: function () {
  }
},
created() {
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