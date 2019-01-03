<template>
  <div class="room-lobby">
    <!-- TODO: delete this -->
    Room Lobby with roomIdInt {{ roomIdInt }} and userId {{ userId }}.
    Number of players: {{ numOfPlayers }}.
    Room State: {{ roomState }}
    <button v-on:click="sendStartGame">Start Game</button>
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
          id: this.roomIdInt,
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
      roomIdInt: Number.isInteger(this.roomId) ? this.roomId : parseInt(this.roomId, 10),
      numOfPlayers: 0,
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
            roomKey: this.roomIdInt,
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
</style>