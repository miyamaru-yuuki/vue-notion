<template>
  <div class="main-page">
    <div class="left-menu" @click.self="onEditNoteEnd()">
      <!-- ノートリスト -->
      <draggable v-bind:list="noteList" group="notes">
        <NoteItem
            v-for="note in noteList"
            v-bind:note="note"
            v-bind:key="note.id"
            v-bind:layer="1"
            @delete="onDeleteNote"
            @select="onSelectNote"
            @editStart="onEditNoteStart"
            @editEnd="onEditNoteEnd"
            @addChild="onAddChildNote"
            @addNoteAfter="onAddNoteAfter"
        />
      </draggable>

      <!-- ノート追加ボタン -->
      <button class="transparent" @click="onClickButtonAdd">
        <i class="fas fa-plus-square"></i>ノートを追加
      </button>
    </div>
    <div class="right-view" @click.self="onEditNoteEnd()">
      右ビュー
    </div>
  </div>
</template>

<script>
import NoteItem from '@/components/parts/NoteItem.vue'
import draggable from 'vuedraggable'
export default {
  data() {
    return {
      noteList : [],
      selectedNote : null,
    }
  },
  methods: {
    onAddNoteCommon : function(targetList, layer, index) {
      layer = layer || 1;
      const note = {
        id : new Date().getTime().toString(16),
        name : `新規ノート-${layer}-${targetList.length}`,
        mouseover : false,
        editing : false,
        selected : false,
        children : [],
        layer : layer,
      };
      if (index == null) {
        targetList.push(note);
      } else {
        targetList.splice(index + 1, 0, note);
      }
    },
    onClickButtonAdd : function() {
      this.onAddNoteCommon(this.noteList);
    },
    onDeleteNote : function(parentNote, note) {
      const targetList = parentNote == null ? this.noteList : parentNote.children;
      const index = targetList.indexOf(note);
      targetList.splice(index, 1);
    },
    onEditNoteStart : function(editNote, parentNote) {
      const targetList = parentNote == null ? this.noteList : parentNote.children;
      for (let note of targetList) {
        note.editing = (note.id === editNote.id);
        this.onEditNoteStart(editNote, note);
      }
    },
    onEditNoteEnd : function(parentNote) {
      const targetList = parentNote == null ? this.noteList : parentNote.children;
      for (let note of targetList) {
        note.editing = false;
        this.onEditNoteEnd(note);
      }
    },
    onAddChildNote : function(note) {
      this.onAddNoteCommon(note.children, note.layer + 1);
    },
    onAddNoteAfter : function(parentNote, note) {
      const targetList = parentNote == null ? this.noteList : parentNote.children;
      const layer = parentNote == null ? 1 : note.layer;
      const index = targetList.indexOf(note);
      this.onAddNoteCommon(targetList, layer, index);
    },
    onSelectNote : function(targetNote) {
      // 再帰的にノートの選択状態を更新
      const updateSelectStatus = function(targetNote, noteList) {
        for (let note of noteList) {
          note.selected = (note.id === targetNote.id);
          updateSelectStatus(targetNote, note.children);
        }
      }
      updateSelectStatus(targetNote, this.noteList);

      // 選択中ノート情報を更新
      this.selectedNote = targetNote;
    },
  },
  components: {
    NoteItem,
    draggable,
  },
}
</script>

<style scoped lang="scss">
.main-page {
  display: flex;
  height: calc(100vh - 60px);
  .left-menu {
    width: 350px;
    background-color: #f7f6f3;
  }
  .right-view {
    flex-grow: 1;
    padding: 10px;
  }
}
</style>