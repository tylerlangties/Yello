<template>
  <form>
    <div draggable="false" class="modal-card" style="width: auto">
      <section class="modal-card-body">
        <!-- Header and Title edit -->

        <div class="content card-header">
          <b-icon pack="fas" icon="chalkboard"></b-icon>

          <div class="card-title">
            <h2 v-if="!detail.nameEditMode">{{detail.name}}</h2>
            <a v-if="!detail.nameEditMode" @click="nameEditMode">
              <b-icon pack="fas" icon="edit" size="small"></b-icon>
            </a>
            <div v-if="detail.nameEditMode" class="name-editmode">
              <b-input type="text" v-model="detail.name"></b-input>

              <button @click="updateCard" class="button is-primary">Save</button>
            </div>
            <span>in {{parent}} list</span>
          </div>
          <p v-if="detail.error" class="error">Your card must have a title</p>
        </div>

        <!-- Description -->

        <div class="columns">
          <div class="column is-three-quarters horizontal">
            <b-icon pack="fas" icon="align-left"></b-icon>
            <div class="description">
              <b-field label="Description">
                <div>
                  <b-input
                    v-if="detail.descriptionEditMode"
                    v-model="detail.description"
                    placeholder="Leave a more detailed description"
                    type="textarea"
                  ></b-input>

                  <a
                    v-if="detail.descriptionEditMode"
                    class="button is-primary"
                    @click="closeEditMode"
                  >Save</a>
                  <div class="noEdit" v-if="!detail.descriptionEditMode">
                    <p>{{detail.description}}</p>
                    <a @click="editMode">
                      <b-icon pack="fas" icon="edit" size="small"></b-icon>
                    </a>
                  </div>
                </div>
              </b-field>
            </div>
          </div>

          <!-- Action buttons Column -->

          <div class="column action-buttons">
            <h5>Add to card</h5>
            <button @click="generateChecklist" class="button is-primary remove">Checklist</button>
            <button @click="generateDatepicker" class="button is-primary move">Due Date</button>
          </div>
        </div>
        <div class="columns">
          <div ref="container" class="column is-three-quarters">
            <Datepicker v-on:deleteDatepicker="deleteDatepicker" v-if="detail.hasDatepicker"></Datepicker>
            <Checklist v-on:deleteChecklist="deleteChecklist" v-if="detail.hasChecklist"></Checklist>
          </div>
          <div class="column is-one-quarter action-buttons">
            <h5>Actions</h5>
            <button class="button is-primary move">Move</button>
            <button v-on:click="deleteCard" class="button is-danger remove">Delete</button>
          </div>
        </div>
      </section>
      <footer class="modal-card-foot">
        <button class="button" type="button" @click="$parent.close()">Close</button>
      </footer>
    </div>
  </form>
</template>

<script>
import Vue from "vue";
import Checklist from "@/components/Card/Checklist.vue";
import Datepicker from "@/components/Card/Datepicker.vue";
export default {
  props: {
    card: {
      type: Object,
      required: true
    },
    parent: {
      type: String
    }
  },
  components: {
    Checklist,
    Datepicker
  },
  data() {
    return {
      detail: {
        name: "",
        nameEditMode: false,
        descriptionEditMode: true,
        description: "",
        error: false,
        hasChecklist: false,
        hasDatepicker: false
      }
    };
  },
  methods: {
    generateChecklist() {
      event.preventDefault();
      this.detail.hasChecklist = true;
    },
    deleteChecklist() {
      this.detail.hasChecklist = false;
    },
    generateDatepicker() {
      event.preventDefault();
      this.detail.hasDatepicker = true;
    },
    deleteDatepicker() {
      this.detail.hasDatepicker = false;
    },
    editMode() {
      this.detail.descriptionEditMode = true;
    },
    closeEditMode() {
      this.detail.descriptionEditMode = false;
      this.$emit("updateCard", this.detail);
    },
    nameEditMode() {
      this.detail.nameEditMode = true;
    },
    deleteCard: function() {
      event.preventDefault();
      this.$parent.close();
      this.$emit("deleteCard", this.boxId);
    },
    updateCard() {
      if (this.detail.name !== "") {
        this.detail.nameEditMode = false;
        this.detail.error = false;
      } else {
        this.detail.error = true;
      }
      this.$emit("updateCard", this.detail);
      console.log("first");
    }
  },
  created() {
    if (this.card.details.description) {
      this.detail = this.card.details;
    } else {
      this.detail.name = this.card.details.name;
    }
  }
};
</script>

<style scoped lang="scss">
.modal-card-body {
  @media (max-width: 992px) {
    width: 100vw;
  }
  color: black;
  width: 80vw;
  height: 80vh;
  .noEdit {
    display: flex;
    .icon {
      margin-left: 1rem;
    }
  }
  .card-header {
    box-shadow: none;
    display: flex;
    .card-title {
      display: flex;
      align-items: flex-end;
      span {
        margin-left: 0.25rem;
      }
    }
    h2 {
      margin: 0;
    }
    .icon {
      margin-right: 1rem;
      margin-top: auto;
    }
  }
  .horizontal {
    display: flex;
    .icon {
      margin-right: 1rem;
    }
    .description {
      width: 100%;
    }
  }
}
.action-buttons {
  button {
    width: 136px;
    margin-bottom: 0.5rem;
  }
}
.error {
  display: flex;
  color: red;
}
.name-editmode {
  display: flex;
  button {
    font-size: 1rem;
  }
}
</style>
