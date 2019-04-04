<template>
  <form>
    <div draggable="false" class="modal-card" style="width: auto">
      <section class="modal-card-body">
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
        <div class="columns">
          <div class="column is-three-fifths horizontal">
            <b-icon pack="fas" icon="align-left"></b-icon>
            <div class="description">
              <b-field label="Description">
                <div>
                  <b-input
                    v-if="detail.editMode"
                    v-model="detail.description"
                    placeholder="Leave a more detailed description"
                    type="textarea"
                  ></b-input>

                  <a v-if="detail.editMode" class="button is-primary" @click="closeEditMode">Save</a>
                  <div class="noEdit" v-if="!detail.editMode">
                    <p>{{detail.description}}</p>
                    <a @click="editMode">
                      <b-icon pack="fas" icon="edit" size="small"></b-icon>
                    </a>
                  </div>
                </div>
              </b-field>
            </div>
          </div>
        </div>
      </section>
      <footer class="modal-card-foot">
        <button v-on:click="deleteCard">Remove me</button>
        <button class="button" type="button" @click="$parent.close()">Close</button>
      </footer>
    </div>
  </form>
</template>

<script>
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
  data() {
    return {
      detail: {
        name: "",
        nameEditMode: false,
        editMode: true,
        description: "",
        error: false
      }
    };
  },
  methods: {
    editMode() {
      this.detail.editMode = true;
    },
    closeEditMode() {
      this.detail.editMode = false;
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
  color: black;
  height: 60vh;
  width: 80vw;
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
