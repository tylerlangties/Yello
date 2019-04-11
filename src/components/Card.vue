<template>
  <div
    class="card"
    :class="{ 'invisible': isHeld }"
    :id="cardId"
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
      <div>{{card.cardInfo.name}}</div>
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
      cardId: this.id,
      isHeld: false,
      isComponentModalActive: false,
      savedData: null
    };
  },
  methods: {
    updateCard(cardInfo) {
      this.$emit("updateCard", [cardInfo, this.cardId]);
    },
    deleteCard: function() {
      this.$emit("deleteCard", this.cardId);
    },
    dragstartHandler: function(event) {
      setTimeout(() => (this.isHeld = true), 0);
      event.dataTransfer.setData("text/plain", event.target.id);
      event.dropEffect = "move";
    },
    dragEndHandler(event) {
      this.isHeld = false;
      this.$emit("cardMoved", this.cardId);
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

<style lang="scss" scoped>
.card {
  cursor: pointer;
  transition: all 100ms ease;
  margin-top: 0.5rem;
  .card-top {
    color: white;
    display: flex;
    background-color: salmon;
    padding: 0.25rem;
    justify-content: space-between;
  }
}
.invisible {
  opacity: 0;
}
</style>
