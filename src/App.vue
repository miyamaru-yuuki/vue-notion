<template>
  <div id="app">
    <h3>ノート一覧</h3>
    <form class="search-form" v-on:submit.prevent="searchNote">
      検索ワード <input type="text" v-model="searchKeyWord">
      <button type="submit">検索</button>
    </form>
    <button v-on:click="openAddModal">ノートを追加する</button>
    <addmodal v-show="showAddModal" v-on:from-child_addNote="addNote" v-on:from-child_delNote="delNote" v-on:from-child_close="closeAddModal"/>
    <tr v-for="(note, index) in notes" :key="index">
      <td>{{ note.noteName }}</td>
      <td><button id="updButton" v-on:click="openEditModal(notes,index)">編集</button></td>
    </tr>
    <editmodal v-if="showEditModal" v-bind:notes="notes" v-bind:index="index" v-on:from-child_editNote="editNote" v-on:from-child_delNote="delNote" v-on:from-child_close="closeEditModal"/>
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
      this.notes.push({"id":id,"noteName":noteName})
      this.closeAddModal();
    });
  },
  editNote: function (noteName) {
    axios.post("http://127.0.0.1:8000/api/note/"+this.notes[this.index].id, {
      _method: 'PUT',
      editNoteName: noteName
    }).then(() => {
      const note = {"id":this.index + 1,"noteName":noteName};
      this.notes.splice(this.index, 1, note)
      this.closeEditModal();
    });
  },
  delNote: function () {
    axios.post('http://127.0.0.1:8000/api/note/' +this.notes[this.index].id,{
      _method: 'DELETE'
    }).then(()=>{
      this.notes.splice(this.index, 1)
      this.closeEditModal();
    })
  },
  searchNote: function() {
    if(!this.searchKeyWord){
      this.getNotes();
      return
    }
    axios.get('http://127.0.0.1:8000/api/search/' +this.searchKeyWord).then((res)=>{
      this.notes = res.data
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