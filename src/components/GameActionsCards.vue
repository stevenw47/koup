<template>
  <div class="game-actions-cards">
    <div class="card-action">
      <button class="card-action-btn" v-on:click="setMode('swap')">
        Swap<span v-if="cardsMode == 'swap'">ping</span>
        <br>
        🔄
      </button>
    </div>
    <div class="card-action">
      <button class="card-action-btn" v-on:click="setMode('kill')">
        Kill<span v-if="cardsMode == 'kill'">ing</span>
        <br>
        🗡️
      </button>
    </div>
    <div class="card-action">
      <button class="card-action-btn" v-on:click="setMode('reveal')">
        Reveal<span v-if="cardsMode == 'reveal'">ing</span>
        <br>
        💡
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['cardsMode'],
  data: function () {
    return {
      actionsInfo: {
        
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
  font-size: 20px;
  background-color: #EEEEEE;
  border: 1.5px solid #BDBDBD;
  border-radius: 7px;

  height: 80%;
  width: 80%;
}
</style>