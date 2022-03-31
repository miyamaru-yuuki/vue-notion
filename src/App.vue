<template>
  <div id="app">
    <h3>ノート一覧</h3>
    <form class="search-form" v-on:submit.prevent="searchNote">
      検索ワード <input type="text" v-model="searchKeyWord">
      <button type="submit">検索</button>
    </form>
    <form class="add-form" v-on:submit.prevent="addNote">
      ノート名 <input type="text" v-model="noteName">
      <button type="submit">追加</button>
    </form>
    <tr v-for="(note, index) in this.notes" :key="index">
      <td v-if="!note.isActive">{{ note.noteName }}</td>
      <input type="text" v-if="note.isActive" v-model="note.noteName">
      <td v-if="!note.isActive"><button id="updButton" v-on:click="updNote(index)">編集</button></td>
      <td v-if="note.isActive"><button id="updfinishButton" v-on:click="updNoteConfirm(note.noteName,index)">編集確定</button></td>
      <td><button id="delButton" v-on:click="delNote(index)">削除</button></td>
    </tr>
  </div>
</template>

<script>
import axios from "axios";

export default {
data: function () {
return {
  notes: [],
  noteName: '',
  editNoteName: '',
  searchKeyWord: ''
}
},
methods: {
  getNotes() {
    axios.get("http://127.0.0.1:8000/api/note")
        .then((res) => {
          this.setNoteDataWithDeactive(res.data);
        });
  },
  addNote: function () {
    axios.post("http://127.0.0.1:8000/api/note", {
      noteName: this.noteName
    }).then((response) => {
      let id = response.id
      this.notes.push({"id":id,"isActive":false,"noteName":this.noteName})
    });
  },
  updNote: function (index) {
    this.$set(this.notes[index], "isActive", true);
  },
  updNoteConfirm: function (editNoteName,index) {
    axios.post("http://127.0.0.1:8000/api/note/"+this.notes[index].id, {
      _method: 'PUT',
      editNoteName: editNoteName
    }).then(() => {
      this.$set(this.notes[index], "isActive", false);
    });
  },
  delNote: function (index) {
    axios.post('http://127.0.0.1:8000/api/note/' +this.notes[index].id,{
      _method: 'DELETE'
    }).then(()=>{
      this.notes.splice(index, 1)
    })
  },
  searchNote: function() {
    if(!this.searchKeyWord){
      this.getNotes();
      return
    }
    axios.get('http://127.0.0.1:8000/api/search/' +this.searchKeyWord).then((res)=>{
      this.setNoteDataWithDeactive(res.data);
    })
  },
  setNoteDataWithDeactive: function(noteData) {
    this.notes = [];
    noteData.forEach((note) => {
      note.isActive = false
    })
    this.notes = noteData;
  },
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