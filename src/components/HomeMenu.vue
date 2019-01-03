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
import gql from 'graphql-tag';

export default {
  // name: 'HomeMenu',
  data: function () {
    return {
      roomId: '', // FIXME: should be an int
    };
  },
  methods: {
    createGame() {
      this.$apollo.mutate({
        // Query
        mutation: gql`mutation ($input: CreateRoomInput!) {
          createRoom(input: $input) {
            room {
              key
            }
          }
        }`,
        variables: {
          input: {}, // empty input
        },
      }).then((res) => {
        // Result
        // TODO: better ui here
        this.roomId = parseInt(res.data.createRoom.room.key, 10); // get roomId from data
        this.joinGame();
      }).catch((error) => {
        // Error
        console.error(error);
        alert("An error has occurred.");
      });
      // this.roomId = 'newRoomId';
      // this.joinGame();
    },
    joinGame() {
      if (this.roomId.length != 0) {
        this.$apollo.mutate({
          mutation: gql`mutation ($input: RoomActionInput!) {
            joinChecker(input: $input) {
              room {
                key
              }
            }
          }`,
          variables: {
            input: {
              roomKey: this.roomId,
            },
          },
        }).then((res) => {
          this.$router.push({ name: 'room', params: { roomId: this.roomId } });
        }).catch((error) => {
          // FIXME: nice ui for error (e.g. no such room or room is not in lobby or full capacity)
          console.error(error);
          alert("An error has occurred.");
        });
      } else {
        // FIXME: make a nice ui pop-up?
        alert("no room id");
      }
    },
  },
};
</script>

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