<template>
  <div class="prompt">
    <div class="content"></div>
    <div class="datepicker">
      <b-icon class="calendar-icon" pack="far" icon="calendar-alt"></b-icon>
      <b-field label="Select a date">
        <b-datepicker
          :min-date="minDate"
          @input="updateDate"
          v-model="dueDate"
          placeholder="Click to select..."
          icon="calendar-alt"
        ></b-datepicker>
      </b-field>
      <button @click="deleteDatepicker" class="delete-datepicker button is-primary">Delete Due date</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    date: {
      Date
    }
  },
  data() {
    return {
      dueDate: new Date(),
      minDate: new Date(-1)
    };
  },
  methods: {
    updateDate() {
      this.$emit("updateDate", this.dueDate);
    },
    deleteDatepicker() {
      this.$emit("deleteDatepicker");
    }
  },
  mounted() {
    if (this.date) {
      this.dueDate = this.date;
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
