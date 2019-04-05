<template>
  <section>
    <div class="checklist-title">
      <b-icon pack="far" icon="check-square" size="small"></b-icon>
      <h4>{{title}}</h4>
      <button @click="deleteChecklist" class="button is-primary">Delete Checklist</button>
    </div>
    <div class="progress-bar">
      <p>{{progress}}%</p>
      <progress class="progress is-primary" :value="progress" max="100">15%</progress>
    </div>
    <div v-for="checkbox in checkboxes" :key="checkbox.id">
      <div class="field">
        <a class="edit-checkbox" @click="checkbox.editing = true">
          <b-icon pack="fas" icon="edit" size="small"></b-icon>
        </a>
        <div class="title-field" v-if="checkbox.editing">
          <b-input v-model="checkbox.title"></b-input>
          <button @click="checkbox.editing = false" class="button is-primary">Save</button>
        </div>

        <b-checkbox
          v-if="!checkbox.editing"
          @change.native="updateProgressBar"
          v-model="checkbox.checked"
        >{{checkbox.title}}</b-checkbox>
        <a class="remove-checkbox" @click="removeCheckbox(checkbox.id)">
          <b-icon pack="fas" icon="trash-alt" size="small"></b-icon>
        </a>
      </div>
    </div>
    <button @click="addCheckbox" class="add-checkbox button is-primary">Add Checkbox</button>
  </section>
</template>

<script>
export default {
  name: "Checklist",
  props: {
    detail: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      title: "Checklist",
      checkbox: false,
      checkboxCustom: "Yes",
      checkboxes: [
        { title: "First", checked: false, editing: false, id: this.randomId() }
      ],
      progress: 0
    };
  },
  methods: {
    addCheckbox() {
      event.preventDefault();
      this.checkboxes.push({
        title: "Untitled",
        checked: false,
        editing: false,
        id: this.randomId()
      });
      this.updateProgressBar();
    },
    deleteChecklist() {
      event.preventDefault();
      this.$emit("deleteChecklist");
    },
    removeCheckbox(id) {
      console.log(id);
      this.checkboxes = this.checkboxes.filter(x => x.id !== id);
      this.updateProgressBar();
    },
    randomId: function() {
      return (
        "id-" +
        Math.random()
          .toString(36)
          .substr(2, 16)
      );
    },
    updateProgressBar() {
      let progress = 0;
      let chunk = 100 / this.checkboxes.length;
      this.checkboxes.forEach(function(checkbox) {
        if (checkbox.checked === true) {
          progress += chunk;
        }
      });
      this.progress = progress.toFixed(0);
      this.updateChecklist();
    },
    updateChecklist() {
      this.$emit("updateChecklist", this.checkboxes);
    }
  },
  mounted() {
    if (this.detail.checklist) {
      this.checkboxes = this.detail.checklist;
    }
    this.updateProgressBar();
    this.updateChecklist();
  }
};
</script>

<style lang="scss" scoped>
.checklist-title {
  display: flex;
  margin-bottom: 1rem;
  span {
    margin-right: 1rem;
  }
  h4 {
    margin: 0;
  }
  button {
    margin-left: auto;
  }
}
.remove-checkbox {
  color: darkgray;
  &:hover {
    color: grey;
  }
}
.progress-bar {
  transition: all 400ms ease;
  display: flex;
  flex-direction: row;
  p {
    font-size: 0.7rem;
    margin-right: 1rem;
    width: 1.5rem;
  }
}
.add-checkbox {
  margin: 1rem 2.5rem;
}
.title-field {
  display: flex;
  width: 100%;
  input {
    width: 20rem;
  }
}
.field {
  display: flex;
  padding: 0.25rem;
  border-radius: 10px;
  .checkbox {
    width: 100%;
  }
  &:hover {
    background-color: whitesmoke;
  }
  .edit-checkbox {
    color: darkgray;
    margin-right: 1rem;
    &:hover {
      color: grey;
    }
  }
}
</style>
