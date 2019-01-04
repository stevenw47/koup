<template>
  <div class="game-cards-card" v-on:click="clickedCard(card.id)">
    <div class="card-content">
      <p v-if="!card.hidden || !card.alive">{{ card.type }}</p>
      <p v-else>HIDDEN</p>
      <p v-if="card.selected">SELECTED</p>
      <p v-if="!card.alive">DEAD</p>
    </div>
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
.game-cards-card {
  /* top/bottom left/right */
  /*margin: 5% 20%;*/
  height: 0;
  padding-top: 56.25%;
  background-color: red;
  position: relative;
}

.card-content {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>