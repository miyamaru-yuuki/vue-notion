<template>
  <div class="directive-page">
    <h2>学習2：ディレクティブ</h2>
    <!-- Vue.js -->
    <h3><span class="badge badge-secondary">Vue.js</span></h3>
    <input type="checkbox" v-model="checked">
    <span v-show="checked">チェック時だけ見えるよ</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      checked : true
    }
  }
}
</script>

<style scoped>
.directive-page {
  padding: 20px;
}
</style>