<template>
  <div :class="isVisible">
    <b-field label="Title">
      <b-input v-model="cardTitle"></b-input>
    </b-field>
    <p :class="titleError">Card must have a valid title</p>
    <div class="options-wrapper">
      <a @click="createCard" class="button is-primary">Save</a>
      <a @click="closeComposer">
        <b-icon pack="fa" icon="times-circle" size="small"></b-icon>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    open: {
      type: Boolean
    }
  },
  data() {
    return {
      cardTitle: "",
      error: false
    };
  },
  methods: {
    createCard() {
      if (this.cardTitle === "") {
        this.error = true;
      } else {
        this.error = false;
        this.$emit("newCard", this.cardTitle);
        this.cardTitle = "";
      }
    },
    closeComposer() {
      this.error = false;
      this.cardTitle = "";
      this.$emit("close");
    }
  },
  computed: {
    isVisible() {
      if (this.open) {
        return "show";
      } else {
        return "hide";
      }
    },
    titleError() {
      if (this.error) {
        return "show";
      } else {
        return "hide";
      }
    }
  }
};
</script>

<style lang="scss" >
.show {
  display: block;
  background-color: whitesmoke;
  padding: 0.5rem;
}
.hide {
  display: none;
}
.options-wrapper {
  display: flex;
  justify-content: space-between;
}
</style>
