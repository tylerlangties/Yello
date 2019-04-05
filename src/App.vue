<template>
  <div id="app">
    <Header></Header>
    <ListWrapper></ListWrapper>
  </div>
</template>

<script>
import ListWrapper from "@/components/ListWrapper.vue";
import List from "@/components/List.vue";
import Header from "@/components/Header.vue";
import "../dragPolyfill.js";
export default {
  data() {
    return {
      id: null,
      lists: [
        { name: "Todo", id: 123 },
        { name: "Doing", id: 234 },
        { name: "Done", id: 555 }
      ]
    };
  },
  components: {
    ListWrapper,
    Header
  },
  methods: {}
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
