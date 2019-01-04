<template>
  <div class="room-lobby">
    <div class="item">
      <p>Game ID:</p>
      <h4>{{ roomId }}</h4>
    </div>
    <div class="item">
      <p>Number of Players:</p>
      <h4>{{ numOfPlayers }}</h4>
    </div>
    <div class="item">
      <button class="menu-btn" v-on:click="sendStartGame">Start Game</button>
    </div>
  </div>
</template>

<script>
import gql from 'graphql-tag';

export default {
  props: ['roomId', 'userId'],
  apollo: {
    room: {
      query: gql`query ($id: ID!) {
        room(id: $id) {
          state
          players {
            edges {
              node {
                id
              }
            }
          }
        }
      }`,
      variables() {
        return {
          id: this.roomId,
        };
      },
      result(res) {
        this.numOfPlayers = res.data.room.players.edges.length;
        this.roomState = res.data.room.state;
      },
      pollInterval: 1000, // ms
    },
  },
  data: function () {
    return {
      numOfPlayers: null,
      roomState: 1, // 1 is lobby
    };
  },
  methods: {
    sendStartGame() {
      this.$apollo.mutate({
        mutation: gql`mutation ($input: RoomActionInput!) {
          startRoom(input: $input) {
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
      }).catch((error) => {
        // FIXME: nice ui here for error
        console.error(error);
        alert("An error has occurred.");
      });
    },
    startGame() {
      this.$emit('change-room-component', 'RoomGame');
    },
  },
  watch: {
    roomState: function (newState, oldState) {
      if (newState == 2) { // game is running
        this.startGame();
      }
    },
  },
};
</script>

<style scoped>
.room-lobby {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.item {

}
p {
  margin-bottom: 0.25em;
}
h4 {
  margin-top: 0.25em;
}
/*TODO: put menu-btn elsewhere (copied from HomeMenu.vue)*/
.menu-btn {
  font-family: 'Ubuntu', sans-serif;
  font-size: 20px;
  height: 2em;
  width: 9em;
  background-color: #EEEEEE;
  border: 1.5px solid #BDBDBD;
  border-radius: 7px;
}
</style>