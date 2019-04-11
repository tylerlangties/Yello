<template>
  <form>
    <div draggable="false" class="modal-card" style="width: auto">
      <section class="modal-card-body">
        <!-- Header and Title edit -->

        <div class="content card-header">
          <b-icon pack="fas" icon="chalkboard"></b-icon>

          <div class="card-title">
            <h2 v-if="!cardInfo.nameEditMode">{{cardInfo.name}}</h2>
            <a v-if="!cardInfo.nameEditMode" @click="nameEditMode">
              <b-icon pack="fas" icon="edit" size="small"></b-icon>
            </a>
            <div v-if="cardInfo.nameEditMode" class="name-editmode">
              <b-input type="text" v-model="cardInfo.name"></b-input>

              <button @click="updateCard" class="button is-primary">Save</button>
            </div>
            <span>in {{parent}} list</span>
          </div>
          <p v-if="cardInfo.error" class="error">Your card must have a title</p>
        </div>

        <!-- Description -->

        <div class="columns">
          <div class="column is-three-quarters horizontal">
            <b-icon pack="fas" icon="align-left"></b-icon>
            <div class="description">
              <b-field label="Description">
                <div>
                  <b-input
                    v-if="cardInfo.descriptionEditMode"
                    v-model="cardInfo.description"
                    placeholder="Leave a more detailed description"
                    type="textarea"
                  ></b-input>

                  <a
                    v-if="cardInfo.descriptionEditMode"
                    class="button is-primary"
                    @click="closeDescriptionEditMode"
                  >Save</a>
                  <div class="noEdit" v-if="!cardInfo.descriptionEditMode">
                    <p>{{cardInfo.description}}</p>
                    <a @click="descriptionEditMode">
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
            <Datepicker
              v-on:deleteDatepicker="deleteDatepicker"
              v-on:updateDate="updateDatePicker"
              v-if="cardInfo.hasDatepicker"
              :date="cardInfo.dueDate"
            ></Datepicker>
            <Checklist
              v-on:deleteChecklist="deleteChecklist"
              v-on:updateChecklist="updateChecklist"
              v-if="cardInfo.hasChecklist"
              :cardInfo="cardInfo"
            ></Checklist>
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
      cardInfo: {
        name: "",
        nameEditMode: false,
        descriptionEditMode: true,
        description: "",
        error: false,
        hasChecklist: false,
        checklist: null,
        hasDatepicker: false,
        dueDate: null
      }
    };
  },
  methods: {
    generateChecklist() {
      event.preventDefault();
      if (!this.cardInfo.hasChecklist) {
        this.cardInfo.hasChecklist = true;
        this.$emit("updateCard", this.cardInfo);
      }
    },
    deleteChecklist() {
      this.cardInfo.hasChecklist = false;
      this.cardInfo.checklist = null;
      this.$emit("updateCard", this.cardInfo);
    },
    updateChecklist(newChecklist) {
      this.cardInfo.checklist = newChecklist;
      this.$emit("updateCard", this.cardInfo);
    },
    generateDatepicker() {
      event.preventDefault();
      this.cardInfo.hasDatepicker = true;
      this.$emit("updateCard", this.cardInfo);
    },
    updateDatePicker(dueDate) {
      this.cardInfo.dueDate = dueDate;
      this.$emit("updateCard", this.cardInfo);
    },
    deleteDatepicker() {
      this.cardInfo.hasDatepicker = false;
      this.cardInfo.dueDate = null;
      this.$emit("updateCard", this.cardInfo);
    },
    descriptionEditMode() {
      this.cardInfo.descriptionEditMode = true;
    },
    closeDescriptionEditMode() {
      this.cardInfo.descriptionEditMode = false;
      this.$emit("updateCard", this.cardInfo);
    },
    nameEditMode() {
      this.cardInfo.nameEditMode = true;
    },
    deleteCard: function() {
      event.preventDefault();
      this.$parent.close();
      this.$emit("deleteCard");
    },
    updateCard() {
      if (this.cardInfo.name !== "") {
        this.cardInfo.nameEditMode = false;
        this.cardInfo.error = false;
      } else {
        this.cardInfo.error = true;
      }
      this.$emit("updateCard", this.cardInfo);
    }
  },
  mounted() {
    if (
      Object.keys(this.card.cardInfo).length >=
      Object.keys(this.cardInfo).length
    ) {
      this.cardInfo = this.card.cardInfo;
    } else {
      this.cardInfo.name = this.card.cardInfo.name;
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
  width: 960px;
  height: 80vh;
  .noEdit {
    display: flex;
    .icon {
      margin-left: 1rem;
      @media (max-width: 767px) {
        display: none;
      }
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
      @media (max-width: 767px) {
        display: none;
      }
    }
  }
  .horizontal {
    display: flex;
    .icon {
      margin-right: 1rem;
      @media (max-width: 767px) {
        display: none;
      }
    }
    .description {
      width: 100%;
    }
  }
}
.action-buttons {
  @media (max-width: 767px) {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  button {
    width: 136px;
    margin-bottom: 0.5rem;
    @media (max-width: 767px) {
      width: 100%;
    }
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
.columns {
  margin: 0;
}
</style>
