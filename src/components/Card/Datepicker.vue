<template>
  <div class="prompt">
    <div class="content"></div>
    <div class="datepicker">
      <b-icon class="calendar-icon" pack="far" icon="calendar-alt"></b-icon>
      <b-field label="Select a date">
        <b-datepicker placeholder="Click to select..." icon="calendar-today"></b-datepicker>
      </b-field>
      <button
        @click="deleteDatepicker"
        class="delete-datepicker button is-primary"
      >Delete Datepicker</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      date: new Date()
    };
  },
  methods: {
    deleteDatepicker() {
      this.$emit("deleteDatepicker");
    }
  }
};
</script>

<style scoped lang="scss">
.datepicker {
  display: flex;
  margin-bottom: 1rem;
  .calendar-icon {
    margin-right: 1rem;
  }
  .delete-datepicker {
    margin-left: auto;
  }
}
</style>
