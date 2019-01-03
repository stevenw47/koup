<template>
  <div class="game-actions-cards">
    <!-- FIXME: use loop? -->
    <button class="action-btn" v-on:click="setMode('swap')">
      Swap<span v-if="cardsMode == 'swap'">ing</span>
    </button>
    <button class="action-btn" v-on:click="setMode('kill')">
      Kill<span v-if="cardsMode == 'kill'">ing</span>
    </button>
    <button class="action-btn" v-on:click="setMode('reveal')">
      Reveal<span v-if="cardsMode == 'reveal'">ing</span>
    </button>
  </div>
</template>

<script>
export default {
  props: ['cardsMode'],
  methods: {
    setMode(newMode) {
      this.$emit('set-mode', newMode);
    },
  },
};
</script>

<style scoped>
.game-actions-cards {
  background-color: red;
  display: flex;
}
.action-btn {
  flex: 1;
  margin: 1em;
}
</style>