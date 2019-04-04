<template>
  <div class="content">
    <!-- <h3>{{list.name}}</h3> -->
    <a :class="addCardVisible" @click="openCardComposer">add a card</a>

    <CardComposer v-on:newCard="newCard" v-on:close="close" :open="open"></CardComposer>
  </div>
</template>

<script>
import CardComposer from "@/components/CardComposer.vue";
export default {
  props: {
    list: {
      type: Object,
      required: true
    },
    open: {
      type: Boolean,
      required: true
    }
  },
  components: {
    CardComposer
  },
  methods: {
    newCard(title) {
      this.$emit("newCard", title);
    },
    close() {
      this.$emit("close");
    },
    openCardComposer: function() {
      this.$emit("openComposer");
    }
  },
  computed: {
    addCardVisible: function() {
      if (this.open) {
        return "hidden";
      } else {
        return "shown button is-light";
      }
    }
  }
};
</script>

<style scoped>
.hidden {
  display: none;
}
.shown {
  width: 100%;
  display: block;
  margin-top: auto;
}
.button {
  width: 100%;
  margin-top: auto;
  border-top-left-radius: 0px;
  border-top-right-radius: 0px;
  border-bottom-right-radius: 3;
  border-bottom-left-radius: 3;
}
</style>
