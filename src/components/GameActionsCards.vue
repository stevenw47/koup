<template>
  <div class="game-actions-cards">
    <!-- FIXME: use loop? -->
    <!-- <button class="action-btn" v-on:click="setMode('swap')">
      Swap<span v-if="cardsMode == 'swap'">ing</span>
    </button>
    <button class="action-btn" v-on:click="setMode('kill')">
      Kill<span v-if="cardsMode == 'kill'">ing</span>
    </button>
    <button class="action-btn" v-on:click="setMode('reveal')">
      Reveal<span v-if="cardsMode == 'reveal'">ing</span>
    </button> -->
    <div class="card-action">
      <button class="card-action-btn" v-on:click="setMode('swap')">
        Swap<span v-if="cardsMode == 'swap'">ing</span>
      </button>
    </div>
    <div class="card-action">
      <button class="card-action-btn" v-on:click="setMode('kill')">
        Kill<span v-if="cardsMode == 'kill'">ing</span>
      </button>
    </div>
    <div class="card-action">
      <button class="card-action-btn" v-on:click="setMode('reveal')">
        Reveal<span v-if="cardsMode == 'reveal'">ing</span>
      </button>
    </div>
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
  display: flex;
  justify-content: space-evenly;
}

/*TODO: put this elsewhere (copied from HomeMenu.vue and renamed+modified)*/
.card-action-btn {
  font-family: 'Ubuntu', sans-serif;
  font-size: 20px;
  background-color: #EEEEEE;
  border: 1.5px solid #BDBDBD;
  border-radius: 7px;

  height: 80%;
  width: 80%;
}

.card-action {
  width: 16vw;
  flex: 1 1 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>