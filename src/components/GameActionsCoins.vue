<template>
  <div class="game-actions-coins">
    <div class="coin-action-text">
      <h2>💰: {{ coins }}</h2>
    </div>
    <div class="coin-action-buttons">
      <button
        v-for="amount in addCoinAmounts" :key="amount"
        class="coin-action-btn"
        v-on:click="addCoins(amount)"
        :disabled="!canAddCoins(amount)">
          +{{ amount }}
      </button>
    </div>
    <div class="coin-action-buttons">
      <button
        v-for="amount in removeCoinAmounts" :key="amount"
        class="coin-action-btn"
        v-on:click="removeCoins(amount)"
        :disabled="!canRemoveCoins(amount)">
          -{{ amount }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['coins'],
  data: function () {
    return {
      addCoinAmounts: [1, 2, 3],
      removeCoinAmounts: [2, 3, 7],
    };
  },
  methods: {
    // can't add coins if you have >= 10
    canAddCoins(num) {
      return (this.coins < 10);
    },
    // can always remove 2 coins (stops at 0)
    // can remove 3 if you have enough, and you don't have >= 10
    // otherwise, you need to have at >= num
    canRemoveCoins(num) {
      if (num == 2) return true;
      if (num == 3) return (this.coins >= 3 && this.coins < 10);
      return (this.coins >= num);
    },
    addCoins(num) {
      if (this.canAddCoins(num)) {
        this.$emit('set-coins', this.coins + num);
      }
    },
    removeCoins(num) {
      if (this.canRemoveCoins(num)) {
        var newCoins = this.coins - num;
        this.$emit('set-coins', newCoins >= 0 ? newCoins : 0);
      }
    },
  },
};
</script>

<style scoped>
.game-actions-coins {
  display: flex;
  flex-direction: column;
}

.coin-action-text, .coin-action-buttons {
  display: flex;
  align-items: center;
}

.coin-action-text {
  height: 26%;
  justify-content: center;
}
.coin-action-buttons {
  height: 37%;
  justify-content: space-evenly;
}

/*TODO: put this elsewhere (copied from HomeMenu.vue and renamed+modified)*/
.coin-action-btn {
  font-family: 'Ubuntu', sans-serif;
  font-size: 1.55vw;
  background-color: #EEEEEE;
  border: 1.5px solid #BDBDBD;
  border-radius: 7px;

  height: 10vh;
  width: 10vw;
}

.coin-action-btn:disabled {
  background-color: #ECEFF1;
  border: 1.5px solid #B0BEC5;
}
</style>