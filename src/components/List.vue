<template>
  <div class="content">
    <div class="list.id" id="list">
      <div
        :class="handleChildHover"
        :id="list.id"
        v-on:drop="dropHandler"
        v-on:dragover="dragoverHandler"
        v-on:dragleave="dragLeaveHandler"
        v-on:dragenter="dragEnterHandler"
      >
        <h3>{{list.name}}</h3>
        <Card
          v-for="card in list.cards"
          :card="card"
          :id="card.id"
          :key="card.id"
          :parent="list.name"
          v-on:deleteCard="deleteCard"
          v-on:updateCard="updateCard"
        ></Card>
      </div>
      <ListHeader
        :open="composerIsOpen"
        v-on:newCard="createNewCard"
        v-on:close="closeCardComposer"
        v-on:openComposer="openCardComposer"
        :list="list"
      ></ListHeader>
    </div>
  </div>
</template>

<script>
import Card from "@/components/Card.vue";
import ListHeader from "@/components/ListHeader.vue";
export default {
  props: {
    list: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      id: this.list.id,
      composerIsOpen: false,
      childIsHovering: false
    };
  },
  components: {
    Card,
    ListHeader
  },
  methods: {
    updateCard(cardInfo) {
      cardInfo.push(this.id);
      this.$emit("updateCard", cardInfo);
    },
    createNewCard(cardTitle) {
      this.composerIsOpen = false;
      const newId = this.randomId();
      const newCard = [{ id: newId, cardInfo: { name: cardTitle } }];
      this.$emit("createCard", [this.id, newCard]);
    },
    deleteCard: function(childId) {
      this.$emit("deleteCard", [childId, this.id]);
    },
    closeCardComposer() {
      this.composerIsOpen = false;
    },
    openCardComposer: function() {
      this.composerIsOpen = true;
    },
    dragoverHandler(event) {
      event.preventDefault();
      event.dataTransfer.dropEffect = "move";
    },
    dragLeaveHandler(event) {
      this.childIsHovering = false;
    },
    dragEnterHandler(event) {
      event.preventDefault();
      this.childIsHovering = true;
    },
    dropHandler(event) {
      event.preventDefault();
      this.childIsHovering = false;
      var data = event.dataTransfer.getData("text/plain");
      if (
        event.target.className === "list" ||
        event.target.className === "list hovered"
      ) {
        let targetListId = event.target.id;
        let movedChildId = data;
        this.$emit("cardMoved", [targetListId, movedChildId]);
      } else if (
        event.target.parentElement.className === "list" ||
        event.target.parentElement.className === "list hovered"
      ) {
        let targetListId = event.target.parentElement.id;
        let movedChildId = data;
        this.$emit("cardMoved", [targetListId, movedChildId]);
      } else if (
        event.target.parentElement.parentElement === "list" ||
        event.target.parentElement.parentElement === "list hovered"
      ) {
        let targetListId = event.target.parentElement.parentElement.id;
        let movedChildId = data;
        this.$emit("cardMoved", [targetListId, movedChildId]);
      }
    },
    randomId: function() {
      return (
        "id-" +
        Math.random()
          .toString(36)
          .substr(2, 16)
      );
    }
  },
  computed: {
    handleChildHover() {
      if (this.childIsHovering) {
        return "list hovered";
      } else {
        return "list";
      }
    }
  }
};
</script>

<style lang="scss" scoped>
#list {
  box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
  border-radius: 3px;
  width: 275px;
  @media (max-width: 992px) {
    width: 80vw;
  }
}
.hovered {
  transition: all 200ms ease;
}
.hidden {
  display: none;
}
.list {
  min-height: 85px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
  padding: 0.5rem;
  padding-bottom: 2rem;
  margin-bottom: 0rem !important;
  box-shadow: none !important;
  -webkit-box-shadow: none;
}
</style>
