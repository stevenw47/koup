<template>
  <div class="game-cards-card" v-on:click="clickedCard(card.id)">
    <p v-if="!card.hidden || !card.alive">{{ card.type }}</p>
    <p v-else>HIDDEN</p>
    <p v-if="card.selected">SELECTED</p>
    <p v-if="!card.alive">DEAD</p>
  </div>
</template>

<script>
export default {
  props: ['card'],
  methods: {
    clickedCard(cardId) {
      this.$emit('clicked-card', cardId);
    },
  },
};
</script>

<style scoped>
</style>