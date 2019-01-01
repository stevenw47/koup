<template>
  <div class="room-game">
    <!-- TODO: delete this -->
    <!-- Room Game with roomId {{ roomId }} and userId {{ userId }}. -->
    <div class="row">
      <div class="col">
        <GameActions
          v-bind:coins="coins"
          v-bind:cardsMode="cardsMode"
          v-on:set-coins="setCoins"
          v-on:set-mode="setMode"
        />
      </div>
      <div class="col">
        <GameCards
          v-bind:cards="cards"
          v-bind:cardsMode="cardsMode"
          v-on:clicked-card="clickedCard"
        />
      </div>
    </div>
  </div>
</template>

<script>
import GameActions from '@/components/GameActions.vue';
import GameCards from '@/components/GameCards.vue';

export default {
  components: {
    GameActions,
    GameCards,
  },
  props: ['roomId', 'userId'],
  data: function () {
    return {
      /*
        A card is an object with id, type, hidden, alive, selected e.g.
        {
        id: 12345
        type: 'Contessa',
        hidden: true,
        alive: true,
        selected: false,
        }
      */
      cards: [], // array of cards
      coins: 0,
      // modes are one of ['display', 'swap', 'kill', 'reveal']
      cardsMode: 'display',
    };
  },
  computed: {
    // max number of cards that can be selected
    selectedCap: function() {
      if (this.cardsMode == 'display') {
        return 0;
      } else if (this.cardsMode == 'swap') {
        // count number of cards that are alive
        return this.cards.filter(card => { return (card.alive && card.owned); }).length;
      } else if (this.cardsMode == 'kill') {
        return 1;
      } else if (this.cardsMode == 'reveal') {
        return 1;
      } else {
        alert("An error has occurred.")
      }
    },
    // FIXME: move to methods? or remove and just use getSelectedCards
    // number of cards that are selected
    currentlySelectedAmount: function() {
      return this.cards.filter(card => { return card.selected; }).length;
    },
  },
  created: function () {
    // TODO: get cards from api
    this.cards.push({ id: 1, type: 'Contessa', hidden: true, alive: true, selected: false, owned: true });
    this.cards.push({ id: 2, type: 'Assassin', hidden: true, alive: true, selected: false, owned: true });
    //this.cards.push({ id: 3, type: 'Captain', hidden: true, alive: true, selected: false });
    //this.cards.push({ id: 4, type: 'Duke', hidden: true, alive: true, selected: false });
  },
  methods: {
    setCoins(newCoins) {
      this.coins = newCoins;
    },
    setMode(newMode) {
      if (this.cardsMode == 'display') {
        // start mode
        if (newMode == 'swap') {
          // TODO: call api to get 2 new cards, then add to cards array
          this.cards.push({ id: 3, type: 'Captain', hidden: false, alive: true, selected: false, owned: false });
          this.cards.push({ id: 4, type: 'Ambassador', hidden: false, alive: true, selected: false, owned: false });
          this.unhideAll();
        } else if (newMode == 'kill') {
          // unhide all cards
          this.unhideAll();
        } else if (newMode == 'reveal') {
          // unhide all cards
          this.unhideAll();
        } else {
          alert("An error has occurred.");
        }
        this.cardsMode = newMode;
      } else if (newMode == this.cardsMode) {
        // end mode
        if (this.cardsMode == 'swap') {
          if (this.currentlySelectedAmount == this.selectedCap) {
            // get selected and dead cards
            this.cards = this.cards.filter(card => { return card.selected || !card.alive; });
            this.unselectAll();
            this.hideAll();
            this.ownAll();
            this.cardsMode = 'display';
          }
        } else if (this.cardsMode == 'kill') {
          if (this.currentlySelectedAmount == 0) {
            this.cardsMode = 'display';
            this.hideAll();
          } else {
            if (this.currentlySelectedAmount == this.selectedCap) {
              var selectedCards = this.getSelectedCards();
              // FIXME: wrap into kill function?
              for (var i = 0; i < selectedCards.length; ++i) {
                selectedCards[i].alive = false;
                selectedCards[i].hidden = false;
              }
              this.unselectAll();
              this.hideAll();
              this.cardsMode = 'display';
            }
          }
        } else if (this.cardsMode == 'reveal') {
          if (this.currentlySelectedAmount == 0) {
            this.cardsMode = 'display';
            this.hideAll();
          } else {
            if (this.currentlySelectedAmount == this.selectedCap) {
              var selectedCards = this.getSelectedCards();
              // TODO: call api to get new card, then return old card
              // also remove old card from array and put new card in its position
              var cardIndex = this.cards.indexOf(selectedCards[0]);
              var newCard = { id: 10, type: 'Duke', hidden: true, alive: true, selected: false };
              this.cards.splice(cardIndex, 1, newCard);
              this.unselectAll();
              this.hideAll();
              this.cardsMode = 'display';
            }
          }
        } else {
          alert("An error has occurred.");
        }
      }
    },
    getSelectedCards() {
      return this.cards.filter(card => { return card.selected; });
    },
    doToAll(field, value) {
      for (var i = 0; i < this.cards.length; ++i) {
        this.cards[i][field] = value;
      }
    },
    unhideAll() {
      this.doToAll('hidden', false);
    },
    hideAll() {
      this.doToAll('hidden', true);
    },
    unselectAll() {
      this.doToAll('selected', false);
    },
    ownAll() {
      this.doToAll('owned', true);
    },
    clickedCard(cardId) {
      // FIXME: use something like enum for cardsMode?
      var card = this.cards.find(card => { return card.id == cardId; });
      if (this.cardsMode == 'display') {
        card.hidden = card.hidden ? false : true;
      } else if (this.cardsMode == 'swap' || this.cardsMode == 'kill' || this.cardsMode == 'reveal') {
        // if selected, unselect it
        if (card.selected) {
          card.selected = false;
          return;
        }
        // can't select dead card
        if (!card.alive) return;

        if (this.selectedCap == 1) {
          // unselect all
          this.unselectAll();
          // select card
          card.selected = true;
          return;
        }

        if (this.currentlySelectedAmount < this.selectedCap) {
          card.selected = true;
        }
      } else {
        alert("An error has occurred.");
      }
    },
    swapCards() {

    },
  },
};
</script>

<style scoped>
.row {
  display: flex;
}
.col {
  flex: 1;
}
</style>