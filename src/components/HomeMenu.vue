<template>
  <div class="home-menu">
    <template v-if="!showJoinGameInput">
      <div class="menu-container">
        <div class="item">
          <button class="menu-btn" v-on:click="createGame">Create Game</button>
        </div>
        <div class="item">
          <button class="menu-btn" v-on:click="tryJoinGame">Join Game</button>
        </div>
      </div>
    </template>
    <template v-else>
      <div class="join-container">
        <div class="item">
          <button class="menu-btn-small" v-on:click="unTryJoinGame">
            <i class="fas fa-chevron-left"></i>
          </button>
        </div>
        <div class="item">
          <input class="join-input" placeholder="Game ID" v-model="roomId" v-on:keyup.enter="joinGame" autofocus>
        </div>
        <div class="item">
          <button class="menu-btn" v-on:click="joinGame">Join</button>
        </div>
      </div>
    </template>
  </div>
</template>

<script>
import gql from 'graphql-tag';

export default {
  // name: 'HomeMenu',
  data: function () {
    return {
      roomId: '',
      showJoinGameInput: false,
    };
  },
  methods: {
    createGame() {
      this.$apollo.mutate({
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
        console.log(res);
        this.roomId = res.data.createRoom.room.key; // get roomId from data
        this.joinGame();
      }).catch((error) => {
        // TODO: better ui here
        console.error(error);
        alert("An error has occurred.");
      });
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
    tryJoinGame() {
      this.showJoinGameInput = true;
    },
    unTryJoinGame() {
      this.showJoinGameInput = false;
    },
  },
};
</script>

<style scoped>
/*.home-menu {
  margin-top: 1em;
}*/

.menu-container, .join-container {
  display: flex;
  justify-content: center;
}

.menu-container {
  flex-direction: row;
}

.join-container {
  flex-direction: column;
}

.item {
  margin: 0.5em;
}

.menu-btn {
  font-family: 'Ubuntu', sans-serif;
  /*font-size: 20px;*/
  font-size: 1.55vw;
  height: 2em;
  width: 9em;
  background-color: #EEEEEE;
  border: 1.5px solid #BDBDBD;
  border-radius: 7px;
}

.menu-btn-small {
  font-family: 'Ubuntu', sans-serif;
  /*font-size: 14px;*/
  font-size: 1.2vw;
  height: 2em;
  width: 2.5em;
  background-color: #EEEEEE;
  border: 1.5px solid #BDBDBD;
  border-radius: 7px;
}

.join-input {
  font-family: 'Ubuntu', sans-serif;
  /*font-size: 20px;*/
  font-size: 1.55vw;
  height: 2em;
  width: 16em;
  border: 1.5px solid #BDBDBD;
  border-radius: 7px;
}

p {
  margin: 0;
}
</style>