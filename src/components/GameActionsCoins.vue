<template>
  <div class="game-actions-coins">
    <div class="coin-action">
      <!-- TODO: add class (dynamic?) for disabled buttons -->
      <h4>Coins: {{ coins }}</h4>
    </div>
    <div class="coin-action">
      <button
        v-for="amount in addCoinAmounts" :key="amount"
        class=".coin-action-btn"
        v-on:click="addCoins(amount)"
        :disabled="!canAddCoins(amount)">
          +{{ amount }}
      </button>
    </div>
    <div class="coin-action">
      <button
        v-for="amount in removeCoinAmounts" :key="amount"
        class=".coin-action-btn"
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
    // TODO: use these functions to disable buttons appropiately
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
  background-color: cyan;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}

.coin-action {
  display: flex;
  justify-content: center;
}

.coin-action-btn {
  margin: 0.75em;
}

.coin-action-btn:disabled {
  background-color: red;
}

h4 {
  /*margin: 0;*/
}
/*TODO: don't use button*/
button:disabled {
  background-color: red;
}
</style>