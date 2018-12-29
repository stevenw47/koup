<template>
  <div class="home-menu container">
    <div class="item">
      <button v-on:click="createGame">Create Game</button>
    </div>
    <div class="item">
      <input v-model="roomId">
    </div>
    <div class="item">
      <button v-on:click="joinGame">Join Game</button>
    </div>
  </div>
</template>

<script>
export default {
  // name: 'HomeMenu',
  data() {
    return {
      roomId: '',
    }
  },
  methods: {
    createGame() {
      const newRoomId = 'newRoomId';
      this.$router.push({ name: 'room', params: { roomId: newRoomId } });
    },
    joinGame() {
      //alert(this.gameId.length == 0 ? "no game id" : this.gameId);
      if(this.roomId.length != 0) {
        this.$router.push({ name: 'room', params: { roomId: this.roomId } });
      } else {
        alert('no room id');
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.home-menu {
  margin-top: 1em;
}

.container {
  background-color: pink;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.item {
  margin: 0.5em;
  color: red;
}
</style>