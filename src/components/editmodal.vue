<template>
  <div id="overlay">
    <div id="content">
      <p>ノート編集画面</p>
      ノート名 <input type="text" v-model="noteName">
      <p><button v-on:click="clickEvent_editNote">編集</button></p>
      <p><button v-on:click="clickEvent_delNote">削除</button></p>
      <p><button v-on:click="clickEvent_close">閉じる</button></p>
    </div>
  </div>
</template>

<script>
export default {
  name: "editmodal",
  data: function () {
    return {
      noteName: ""
    }
  },
  methods :{
    clickEvent_editNote: function(){
      this.$emit('from-child_addNote',this.noteName)
      this.noteName = ""
    },
    clickEvent_delNote: function(){
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

</style>