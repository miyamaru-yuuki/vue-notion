<template>
  <div id="overlay">
    <div id="content">
      <p>ノート追加画面</p>
      ノート名 <input type="text" v-model="noteName">
      <p><button v-on:click="clickEvent_addNote">追加</button></p>
      <p><button v-on:click="clickEvent_close">閉じる</button></p>
    </div>
  </div>
</template>

<script>
export default {
  name: "addmodal",
  data: function () {
    return {
      noteName: ""
    }
  },
  methods :{
    clickEvent_addNote: function(){
      this.$emit('from-child_addNote',this.noteName)
      this.noteName = ""
    },
    clickEvent_close: function(){
      this.$emit('from-child_close')
      this.noteName = ""
    }
  }
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