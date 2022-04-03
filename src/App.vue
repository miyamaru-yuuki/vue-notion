<template>
  <div id="app">
    <h3>ノート一覧</h3>
    <form class="search-form" v-on:submit.prevent="searchNote">
      検索ワード <input type="text" v-model="searchKeyWord">
      <button type="submit">検索</button>
    </form>
    <button v-on:click="openAddModal">ノートを追加する</button>
    <addmodal v-show="showAddModal" v-on:from-child_addNote="addNote" v-on:from-child_delNote="delNote" v-on:from-child_close="closeAddModal"/>
    <tr v-for="(note, index) in this.notes" :key="index">
      <td v-if="!note.isActive">{{ note.noteName }}</td>
      <input type="text" v-if="note.isActive" v-model="note.noteName">
      <td v-if="!note.isActive"><button id="updButton" v-on:click="openEditModal(this.notes,index)">編集</button></td>
    </tr>
    <editmodal v-show="showEditModal" v-bind:notes="this.notes" v-bind:index="this.index" v-on:from-child_editNote="editNote(noteName)" v-on:from-child_close="closeEditModal"/>
  </div>
</template>

<script>
import axios from "axios";
import addmodal from '@/components/addmodal.vue';
import editmodal from '@/components/editmodal.vue';

export default {
data: function () {
return {
  notes: [],
  note: '',
  index: 0,
  noteName: '',
  editNoteName: '',
  searchKeyWord: '',
  showAddModal: false,
  showEditModal: false
}
},
methods: {
  getNotes() {
    axios.get("http://127.0.0.1:8000/api/note")
        .then((res) => {
          this.setNoteDataWithDeactive(res.data);
        });
  },
  addNote: function (noteName) {
    axios.post("http://127.0.0.1:8000/api/note", {
      noteName: noteName
    }).then((response) => {
      let id = response.id
      this.notes.push({"id":id,"isActive":false,"noteName":noteName})
      this.closeAddModal();
    });
  },
  editNote: function (noteName) {
    axios.post("http://127.0.0.1:8000/api/note/"+this.notes[this.index].id, {
      _method: 'PUT',
      editNoteName: noteName
    }).then(() => {
      this.$set(this.notes[this.index], "isActive", false);
      this.closeEditModal();
    });
  },
  delNote: function (index) {
    axios.post('http://127.0.0.1:8000/api/note/' +this.notes[index].id,{
      _method: 'DELETE'
    }).then(()=>{
      this.notes.splice(index, 1)
      this.closeEditModal();
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
  openAddModal: function () {
    this.showAddModal = true
  },
  closeAddModal: function () {
    this.showAddModal = false
  },
  openEditModal: function (notes,index) {
    this.index = index
    this.showEditModal = true
  },
  closeEditModal: function () {
    this.showEditModal = false
  },
},
created() {
    this.getNotes();
},
  components: {
    addmodal,
    editmodal
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