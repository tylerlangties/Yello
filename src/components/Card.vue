<template>
  <div
    class="box-2"
    :class="{ 'invisible': isHeld }"
    :id="boxId"
    :draggable="!isComponentModalActive"
    v-on:dragstart="dragstartHandler"
    v-on:dragend="dragEndHandler"
    v-on:dragenter="dragEnterHandler"
    v-on:dragleave="dragLeaveHandler"
  >
    <b-modal :active.sync="isComponentModalActive" has-modal-card>
      <card-detail
        :parent="parent"
        :card="card"
        v-on:updateCard="updateCard"
        v-on:deleteCard="deleteCard"
        draggable="false"
      ></card-detail>
    </b-modal>
    <div class="card-top">
      <div>{{card.details.name}}</div>
      <a @click="isComponentModalActive = true">
        <b-icon pack="fas" icon="edit" size="small"></b-icon>
      </a>
    </div>
  </div>
</template>

<script>
import CardDetail from "@/components/CardDetail.vue";
export default {
  props: {
    id: {
      type: String,
      required: true
    },
    card: {
      type: Object,
      required: true
    }
  },
  components: {
    CardDetail
  },
  data() {
    return {
      parent: null,
      boxId: this.id,
      isHeld: false,
      isComponentModalActive: false,
      savedData: null
    };
  },
  methods: {
    updateCard(details) {
      // this.savedData = details;
      this.$emit("updateCard", [details, this.boxId]);
      console.log("second");
    },
    deleteCard: function() {
      this.$emit("deleteCard", this.boxId);
    },
    dragstartHandler: function(event) {
      setTimeout(() => (this.isHeld = true), 0);
      // Add the target element's id to the data transfer object
      event.dataTransfer.setData("text/plain", event.target.id);
      event.dropEffect = "move";
    },
    dragEndHandler(event) {
      this.isHeld = false;
      this.$emit("cardMoved", this.boxId);
    },
    dragEnterHandler(event) {
      event.preventDefault();
    },
    dragLeaveHandler(event) {}
  },
  mounted() {
    this.parent = this.$attrs.parent;
  }
};
</script>

<style>
.box-2 {
  background-color: salmon;
  cursor: pointer;
  transition: all 100ms ease;
  color: white;
  border-radius: 3px;
  padding: 0.25rem;
  margin-top: 0.5rem;
}
.chore {
  width: 80px;
}
.invisible {
  opacity: 0;
}
.card-top {
  display: flex;
  justify-content: space-between;
}
</style>
