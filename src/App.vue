<template>
  <div id="app">
    <Header></Header>
    <ListWrapper></ListWrapper>
  </div>
</template>

<script>
import Header from "@/components/Header.vue";
import ListWrapper from "@/components/ListWrapper.vue";
export default {
  components: {
    ListWrapper,
    Header
  }
};
</script>

<style lang="scss">
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  height: 100vh;
  width: 100vw;
}
</style>
