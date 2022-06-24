<template>
  <div class="about">
    <h1>This is app-vue-history about page</h1>
    <Demo />
  </div>
</template>
<script>
import Demo from "../../../app-vue-hash/src/components/Demo.vue";
export default {
  components: {
    Demo,
  },
  mounted() {
    window.a = 1;
    console.log(window.a, "app-vue-history");
  },
};
</script>
<style>
/* 测试全局css污染 */
h1 {
  color: red;
}
</style>
