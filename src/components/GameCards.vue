<template>
  <div class="game-cards">
    <GameCardsCard
      class="card"
      v-for="card in cards" :key="card.id"
      v-bind:card="card"
      v-on:clicked-card="clickedCard"
    />
  </div>
</template>

<script>
import GameCardsCard from '@/components/GameCardsCard.vue';
export default {
  components: {
    GameCardsCard,
  },
  // FIXME: don't need cardsMode here?
  props: ['cards', 'cardsMode'],
  methods: {
    clickedCard(cardId) {
      this.$emit('clicked-card', cardId);
    },
  },
};
</script>

<style scoped>
.game-cards {
  height: 100vh;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.card {
  background-color: pink;
  flex: 1 1 50%;
}
</style>