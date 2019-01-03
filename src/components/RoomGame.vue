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
import gql from 'graphql-tag';

import GameActions from '@/components/GameActions.vue';
import GameCards from '@/components/GameCards.vue';

export default {
  components: {
    GameActions,
    GameCards,
  },
  props: ['roomId', 'userId'],
  apollo: {
    allCards: {
      query: gql`query {
        allCards {
          edges {
            node {
              dbId
              type
            }
          }
        }
      }`,
      manual: true,
      skip: true,
      result(res) {
        var rawCardsMap = res.data.allCards.edges;
        for (let i = 0; i < rawCardsMap.length; ++i) {
          let rawCard = rawCardsMap[i].node;
          this.cardsMap[rawCard.dbId] = rawCard.type;
        }
        console.log(this.cardsMap);
        // only fetch player after map is created
        this.$apollo.queries.player.skip = false;
        this.$apollo.queries.player.refetch();
      },
    },
    player: {
      query: gql`query ($id: ID!) {
        player(id: $id) {
          hand
        }
      }`,
      variables() {
        return {
          id: this.userId,
        };
      },
      manual: true,
      skip: true,
      result(res) {
        var rawCards = res.data.player.hand;
        var newCards = this.parseCards(rawCards, { hidden: true, alive: true, selected: false, owned: true });
        this.cards = newCards;
        // console.log(rawPlayerCards);
        // for (let i = 0; i < rawPlayerCards.length; ++i) {
        //   let rawCardDbId = rawPlayerCards[i];
        //   let cardType = this.cardsMap[rawCardDbId];
        //   this.addCard({ id: rawCardDbId, type: cardType, hidden: true, alive: true, selected: false, owned: true });
        // }
      },
    },
  },
  data: function () {
    return {
      /*
        A card is an object with id, type, hidden, alive, selected, owned e.g.
        {
        id: 12345
        type: 'Contessa',
        hidden: true,
        alive: true,
        selected: false,
        owned: true
        }
      */
      cards: [], // array of cards
      coins: 0,
      // modes are one of ['display', 'swap', 'kill', 'reveal']
      cardsMode: 'display',
      cardsMap: {}, // map from card's dbId to type
    };
  },
  computed: {
    // max number of cards that can be selected
    selectedCap: function () {
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
        alert("An error has occurred.");
      }
    },
    // FIXME: move to methods? or remove and just use getSelectedCards
    // number of cards that are selected
    currentlySelectedAmount: function() {
      return this.cards.filter(card => { return card.selected; }).length;
    },
  },
  created: function () {
    this.$apollo.queries.allCards.skip = false;
    this.$apollo.queries.allCards.refetch();
    // TODO: get cards from api
    //this.cards.push({ id: 1, type: 'Contessa', hidden: true, alive: true, selected: false, owned: true });
    //this.cards.push({ id: 2, type: 'Assassin', hidden: true, alive: true, selected: false, owned: true });
    //this.cards.push({ id: 3, type: 'Captain', hidden: true, alive: true, selected: false });
    //this.cards.push({ id: 4, type: 'Duke', hidden: true, alive: true, selected: false });
  },
  methods: {
    parseCards(rawCards, cardProperties) {
      // console.log(rawCards);
      // console.log(cardProperties);
      // FIXME: change all for loop vars to let
      let parsedCards = [];
      for (let i = 0; i < rawCards.length; ++i) {
        // console.log(i);
        // FIXME: should i use object assign or json?
        let card = JSON.parse(JSON.stringify(cardProperties));
        // let card = cardProperties;
        let rawCardDbId = rawCards[i];
        let cardType = this.cardsMap[rawCardDbId];
        // console.log(rawCardDbId);
        card.id = rawCardDbId;
        card.type = cardType;
        // console.log(card);
        parsedCards.push(card);
      }
      // console.log(parsedCards);
      return parsedCards;
    },
    // addCard(newCard) {
    //   console.log(newCard);
    //   this.cards.push(newCard);
    // },
    // create and return a card
    // createCard(id, type, hidden, alive, selected, owned) {
    //   return { id: id, type: type, hidden: hidden, alive: alive, selected: selected, owned: owned};
    // },
    setCoins(newCoins) {
      this.coins = newCoins;
    },
    setMode(newMode) {
      if (this.cardsMode == 'display') {
        // start mode
        if (newMode == 'swap') {
          // FIXME: make and use swapCards function
          // TODO: call api to get 2 new cards, then add to cards array
          this.$apollo.mutate({
            mutation: gql`mutation ($input: GetCardsInput!) {
              swapStart(input: $input) {
                player {
                  hand
                }
              }
            }`,
            variables: {
              input: {
                id: this.userId,
                numCards: 2,
              },
            },
          }).then((res) => {
            // FIXME: check this
            var rawCards =
              res.data.swapStart.player.hand
              .filter(x => { return this.cards.reduce((accumulator, currentValue) => { return accumulator && (x != currentValue.id ) }, true); });
            var newCards = this.parseCards(rawCards, { hidden: false, alive: true, selected: false, owned: false });
            this.cards = this.cards.concat(newCards);

            this.cardsMode = newMode;
            this.unhideAll();
          }).catch((error) => {
            // FIXME: nice ui here for error
            // FIXME: some alert when swap in progress by someone else
            console.error(error);
            alert("An error has occurred.");
          });
          // this.cards.push({ id: 3, type: 'Captain', hidden: false, alive: true, selected: false, owned: false });
          // this.cards.push({ id: 4, type: 'Ambassador', hidden: false, alive: true, selected: false, owned: false });
          // this.unhideAll();
        } else if (newMode == 'kill') {
          // unhide all cards
          this.unhideAll();
          this.cardsMode = newMode;
        } else if (newMode == 'reveal') {
          // TODO: check whether a swap is in progress before doing these 2 things
          // unhide all cards
          this.unhideAll();
          this.cardsMode = newMode;
        } else {
          alert("An error has occurred.");
        }
      } else if (newMode == this.cardsMode) {
        // end mode
        if (this.cardsMode == 'swap') {
          if (this.currentlySelectedAmount == this.selectedCap) {
            // FIXME: don't change order of dead cards when swapping
            // TODO: call api to return cards
            // get unselected and alive cards (cards to return)
            let cardsToReturn = this.cards.filter(card => { return !card.selected && card.alive; });
            console.log(cardsToReturn);
            this.$apollo.mutate({
              mutation: gql`mutation ($input: PutCardsInput!) {
                swapEnd(input: $input) {
                  player {
                    id
                  }
                }
              }`,
              variables: {
                input: {
                  id: this.userId,
                  hand: cardsToReturn.map(x => { return x.id; }),
                },
              },
            }).then((res) => {
              this.cards = this.cards.filter(card => { return card.selected || !card.alive; });
              this.unselectAll();
              this.hideAll();
              this.ownAll();
              this.cardsMode = 'display';
            }).catch((error) => {
              // FIXME: nice ui here for error
              console.error(error);
              alert("An error has occurred.");
            });

            // get selected cards and dead cards (cards to keep)
            // this.cards = this.cards.filter(card => { return card.selected || !card.alive; });
            // this.unselectAll();
            // this.hideAll();
            // this.ownAll();
            // this.cardsMode = 'display';
          }
        } else if (this.cardsMode == 'kill') {
          if (this.currentlySelectedAmount == 0) {
            this.cardsMode = 'display';
            this.hideAll();
          } else {
            if (this.currentlySelectedAmount == this.selectedCap) {
              var selectedCards = this.getSelectedCards();
              // FIXME: wrap into kill function?
              for (let i = 0; i < selectedCards.length; ++i) {
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
              console.log('selectedCards');
              console.log(selectedCards);
              console.log(selectedCards.map(x => { return x.id; }));
              this.$apollo.mutate({
                mutation: gql`mutation ($input: PutCardsInput!) {
                  revealCard(input: $input) {
                    player {
                      hand
                    }
                  }
                }`,
                variables: {
                  input: {
                    id: this.userId,
                    // FIXME: abstract this (used twice!)
                    hand: selectedCards.map(x => { return x.id; }),
                  },
                },
              }).then((res) => {
                // if same cards, don't need to do anything
                // if (res.data.revealCard.player.hand.sort((a, b) => a - b) != this.cards.map(x => { return x.id; }).sort((a, b) => a - b)) {
                // console.log(res.data.revealCard.player.hand.every(this.cards.map(x => { return x.id; })));
                console.log(JSON.stringify(res.data.revealCard.player.hand.sort((a, b) => a - b)) == JSON.stringify(this.cards.map(x => { return x.id; }).sort((a, b) => a - b)));
                // TODO: put comparison elsewhere
                if (!(JSON.stringify(res.data.revealCard.player.hand.sort((a, b) => a - b)) == JSON.stringify(this.cards.map(x => { return x.id; }).sort((a, b) => a - b)))) {
                  // FIXME: abstract this (used twice!)
                  let rawNewCards = res.data.revealCard.player.hand.filter(x => { return this.cards.reduce((accumulator, currentValue) => { return accumulator && (x != currentValue.id ) }, true); });
                  var newCards = this.parseCards(rawNewCards, { hidden: true, alive: true, selected: false, owned: true });
                  // var selectedIndexes = this.cards.filter(x => { return x.selected; }).map((x, index) => { return index; });
                  var selectedIndexes = this.cards.map((x, index) => { return x.selected ? index : -1; }).filter(x => { return x != -1 });
                  for (let i = 0; i < selectedIndexes.length; ++i) {
                    let cardIndex = selectedIndexes[i];
                    this.cards.splice(cardIndex, 1, newCards[i]);
                  }
                }
                this.unselectAll();
                this.hideAll();
                this.cardsMode = 'display';
              }).catch((error) => {

              });
              // var selectedCards = this.getSelectedCards();
              // TODO: call api to get new card, then return old card
              // also remove old card from array and put new card in its position
              // var cardIndex = this.cards.indexOf(selectedCards[0]);
              // var newCard = { id: 10, type: 'Duke', hidden: true, alive: true, selected: false, owned: true };
              // this.cards.splice(cardIndex, 1, newCard);
              // this.unselectAll();
              // this.hideAll();
              // this.cardsMode = 'display';
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
      for (let i = 0; i < this.cards.length; ++i) {
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