<template>
  <div class="game-actions">
    <GameActionsCoins
      class="action"
      v-bind:coins="coins"
      v-on:set-coins="setCoins"
    />
    <GameActionsCards
      class="action"
      v-bind:cardsMode="cardsMode"
      v-on:set-mode="setMode"
    />
  </div>
</template>

<script>
import GameActionsCoins from '@/components/GameActionsCoins.vue';
import GameActionsCards from '@/components/GameActionsCards.vue';
export default {
  components: {
    GameActionsCoins,
    GameActionsCards,
  },
  props: ['coins', 'cardsMode'],
  data: function () {
    return {
    };
  },
  methods: {
    // propagate to parent
    setCoins(newCoins) {
      this.$emit('set-coins', newCoins);
    },
    // propagate to parent
    setMode(newMode) {
      this.$emit('set-mode', newMode);
    },
  },
};
</script>

<style scoped>
.game-actions {
  height: 100vh;
  display: flex;
  flex-direction: column;
}
.action {
  flex: 1;
}
</style>