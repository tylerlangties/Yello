<template>
  <div class="container">
    <div class="lists">
      <List
        :class="list.id"
        v-on:updateCard="updateCard"
        v-on:createCard="createCard"
        v-on:deleteCard="deleteCard"
        v-on:cardMoved="moveCard"
        v-for="list in lists"
        :list="list"
        :key="list.id"
      ></List>
    </div>
  </div>
</template>

<script>
import List from "@/components/List.vue";

export default {
  data() {
    return {
      cardToMove: null,
      listIndex: null,
      cardIndex: null,
      lists: [
        { name: "Todo", id: 123, cards: [] },
        { name: "Doing", id: 234, cards: [] },
        { name: "Done", id: 555, cards: [] }
      ]
    };
  },
  components: {
    List
  },
  methods: {
    moveCard(payload) {
      //Find list index of the container the card is being moved to
      const listIndex = this.getListIndex(payload[0]);
      const cardId = payload[1];
      //Find card and list index of current cards position
      const cardCoords = this.findCardCoords(cardId);
      //Card to be moved
      const newCard = this.lists[cardCoords[1]].cards[cardCoords[0]];
      //Move card to new position
      this.addNewCard(listIndex, newCard);
      //Remove card from prior position
      this.removeCard(cardCoords[1], cardId);
    },
    updateCard(details) {
      let cardDetails = details[0];
      let cardId = details[1];
      let listId = details[2];
      let listIndex = this.getListIndex(listId);
      let foundCard = this.getCardIndex(cardId, listIndex);
      this.lists[listIndex].cards[foundCard].details = cardDetails;
    },
    createCard(details) {
      let listId = details[0];
      let newCard = details[1][0];
      let listIndex = this.getListIndex(listId);
      this.addNewCard(listIndex, newCard);
    },
    deleteCard(deletionCoords) {
      let cardId = deletionCoords[0];
      let listId = deletionCoords[1];
      let listIndex = this.lists.findIndex(x => x.id == listId);
      this.removeCard(listIndex, cardId);
    },

    //helpers
    removeCard(listIndex, cardId) {
      this.lists[listIndex].cards = this.lists[listIndex].cards.filter(
        x => x.id !== cardId
      );
    },
    addNewCard(listIndex, newCard) {
      this.lists[listIndex].cards.push(newCard);
    },
    getListIndex(listId) {
      return this.lists.findIndex(x => x.id == listId);
    },
    getCardIndex(cardId, listIndex) {
      return this.lists[listIndex].cards.findIndex(x => x.id === cardId);
    },
    //returns index of list containing card and index of card
    findCardCoords(cardId) {
      for (let i = 0; i < this.lists.length; i++) {
        let cardIndex = this.lists[i].cards.findIndex(x => x.id === cardId);
        if (cardIndex >= 0) {
          this.listIndex = i;
          this.cardIndex = cardIndex;
          return [cardIndex, i];
        }
      }
    }
  }
};
</script>

<style scoped lang="scss">
.container {
  display: flex;
  justify-content: center;
}
.lists {
  display: grid;
  grid-template-columns: 300px 300px 300px;
  grid-column-gap: 1rem;
  @media (max-width: 767px) {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
  }
}
</style>
