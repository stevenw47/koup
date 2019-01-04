<template>
  <div class="game-cards-card" v-on:click="clickedCard(card.id)">
    <div class="card-content">
      <p v-if="!card.hidden || !card.alive">{{ card.type }}</p>
      <p v-else>HIDDEN</p>
      <p>👑</p>
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
  display: flex;
  justify-content: center;
  align-items: center;
}

.card-content {
  height: 45vh;
  width: 20vw;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  color: #00131a;
  background-color: white;
  border: 0;
  border-radius: 15px;
}

p {
  margin: 0;
}
</style>