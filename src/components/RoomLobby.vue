<template>
  <div class="room-lobby">
    <!-- TODO: delete this -->
    Room Lobby with roomId {{ roomId }} and userId {{ userId }}.
    Number of players: {{ numOfPlayers }}.
    <button v-on:click="startGame">Start Game</button>
  </div>
</template>

<script>
export default {
  props: ['roomId', 'userId'],
  data: function () {
    return {
      numOfPlayers: 0,
    };
  },
  created: function () {
    // TODO: fetch from server
    const vue = this;
    // FIXME: don't use setInterval/clearInterval?

    // var query = setInterval(function(){
    //   ++vue.numOfPlayers;
    //   alert(vue.numOfPlayers);
    //   if (vue.numOfPlayers == 5) {
    //     vue.startGame();
    //     clearInterval(query);
    //   }
    // }, 1000);

    // TODO: start game based on query
    var shouldStartGame = false;
    if (shouldStartGame) this.startGame();
  },
  methods: {
    startGame() {
      this.$emit('change-room-component', 'RoomGame');
    },
  },
};
</script>

<style scoped>
</style>