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
    };
  },
  methods: {
    createGame() {
      // TODO: call api to create room, and retrieve the generated roomId
      // code to get a new room id
      this.roomId =  'newRoomId';
      this.joinGame();
    },
    joinGame() {
      // TODO: call api to join room with specified roomId; if can join, redirect; else, warn user no room/already started
      if (this.roomId.length != 0) {
        // check if room is joinable. if yes, redirect to new url. otherwise, warn user
        var canJoin = true;
        if (canJoin) {
          this.$router.push({ name: 'room', params: { roomId: this.roomId } });
        } else {
          alert("room not joinable");
        }
      } else {
        alert("no room id");
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