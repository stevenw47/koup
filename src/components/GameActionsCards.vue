<template>
  <div class="game-actions-cards">
    <template v-for="actionInfo in actionsInfo">
      <div class="card-action">
        <button class="card-action-btn" v-on:click="setMode(actionInfo.cardsMode)">
          {{ actionInfo.text }}<span v-show="cardsMode == actionInfo.cardsMode">{{ actionInfo.adjectiveSuffix }}</span>
          <br>
          {{ actionInfo.emoji }}
        </button>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  props: ['cardsMode'],
  data: function () {
    return {
      actionsInfo: {
        Swap: {
          cardsMode: 'swap',
          text: 'Swap',
          adjectiveSuffix: 'ping',
          emoji: '🔄',
        },
        Kill: {
          cardsMode: 'kill',
          text: 'Kill',
          adjectiveSuffix: 'ing',
          emoji: '🗡️',
        },
        Reveal: {
          cardsMode: 'reveal',
          text: 'Reveal',
          adjectiveSuffix: 'ing',
          emoji: '💡',
        },
      },
    };
  },
  methods: {
    setMode(newMode) {
      this.$emit('set-mode', newMode);
    },
  },
};
</script>

<style scoped>
.game-actions-cards {
  display: flex;
  justify-content: space-evenly;
}

.card-action {
  width: 16vw;
  flex: 1 1 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

/*TODO: put this elsewhere (copied from HomeMenu.vue and renamed+modified)*/
.card-action-btn {
  font-family: 'Ubuntu', sans-serif;
  font-size: 1.55vw;
  background-color: #EEEEEE;
  border: 1.5px solid #BDBDBD;
  border-radius: 7px;

  height: 80%;
  width: 80%;
}
</style>