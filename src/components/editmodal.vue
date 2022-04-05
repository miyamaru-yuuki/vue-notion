<template>
  <div id="overlay">
    <div id="content">
      <p>ノート編集画面</p>
      ノート名 <input type="text" v-model="noteName">
      <p><button v-on:click="clickEvent_editNote">編集</button></p>
      <p><button v-on:click="clickEvent_delNote(index)">削除</button></p>
      <p><button v-on:click="clickEvent_close">閉じる</button></p>
    </div>
  </div>
</template>

<script>
export default {
  name: "editmodal",
  data: function () {
    return {
      noteName: this.notes[this.index].noteName
    }
  },
  methods :{
    clickEvent_editNote: function(){
      this.$emit('from-child_editNote',this.noteName)
    },
    clickEvent_delNote: function(index){
      console.log(index)
      this.$emit('from-child_delNote',index)
      this.noteName = ""
    },
    clickEvent_close: function(){
      this.$emit('from-child_close')
      this.noteName = ""
    }
  },
  props: ["notes","index"],
  mounted() {
    this.noteName = this.notes[this.index].noteName
  },
}
</script>

<style scoped>
#overlay{
  z-index:1;
  position:fixed;
  top:0;
  left:0;
  width:100%;
  height:100%;
  background-color:rgba(0,0,0,0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}
#content{
  z-index:2;
  width:50%;
  padding: 1em;
  background:#fff;
}
</style>