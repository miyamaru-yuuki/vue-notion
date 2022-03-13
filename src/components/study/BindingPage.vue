<template>
  <div class="binding-page">
    <h2>学習1：双方向バインディング</h2>
    <!-- Vue.js -->
    <h3><span class="badge badge-secondary">Vue.js</span></h3>
    <input class="form-control" v-model="name">
    <span>入力された名前は{{name}}です</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name : ''
    }
  }
}
</script>

<style scoped>
.binding-page {
  padding: 20px;
}
</style>