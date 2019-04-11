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

      //Remove card from prior position
      this.removeCard(cardCoords[1], cardId);
      this.addNewCard(listIndex, newCard);
      this.saveLists();
    },
    updateCard(cardInfo) {
      const card = cardInfo[0];
      const cardId = cardInfo[1];
      const listId = cardInfo[2];
      const listIndex = this.getListIndex(listId);
      const foundCard = this.getCardIndex(cardId, listIndex);
      this.lists[listIndex].cards[foundCard].cardInfo = card;
      this.saveLists();
    },
    createCard(cardInfo) {
      const listId = cardInfo[0];
      const newCard = cardInfo[1][0];
      const listIndex = this.getListIndex(listId);
      this.addNewCard(listIndex, newCard);
      this.saveLists();
    },
    deleteCard(deletionCoords) {
      const cardId = deletionCoords[0];
      const listId = deletionCoords[1];
      const listIndex = this.lists.findIndex(x => x.id == listId);
      this.removeCard(listIndex, cardId);
      this.saveLists();
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
    },

    //Local storage
    saveLists() {
      const parsed = JSON.stringify(this.lists);
      localStorage.setItem("lists", parsed);
    }
  },
  mounted() {
    if (localStorage.getItem("lists")) {
      try {
        this.lists = JSON.parse(localStorage.getItem("lists"));
      } catch (e) {
        localStorage.removeItem("lists");
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
  @media (max-width: 992px) {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
  }
}
</style>
