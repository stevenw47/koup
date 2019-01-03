<template>
  <div class="room">
    <component v-bind:is="currentRoomComponent" v-bind:roomId="roomId" v-bind:userId="userId" v-on:change-room-component="changeRoomComponent"></component>
  </div>
</template>

<script>
import gql from 'graphql-tag';
// @ is an alias to /src
import RoomLobby from '@/components/RoomLobby.vue';
import RoomGame from '@/components/RoomGame.vue';

export default {
  components: {
    RoomLobby,
    RoomGame,
  },
  props: ['roomId'],
  data: function () {
    return {
      currentRoomComponent: '',
      userId: '',
    };
  },
  created: function () {
    this.setup();
  },
  watch: {
    '$route' (to, from) {
      // react to route changes...
      this.setup();
    }
  },
  methods: {
    setup() {
      this.$apollo.mutate({
        mutation: gql`mutation ($input: RoomActionInput!) {
          joinRoom(input: $input) {
            player {
              id
            }
          }
        }`,
        variables: {
          input: {
            roomKey: this.roomId,
          },
        },
      }).then((res) => {
        this.userId = res.data.joinRoom.player.id;
        this.currentRoomComponent = 'RoomLobby';
      }).catch((error) => {
        // FIXME: nice ui here for error
        console.error(error);
        alert("An error has occurred.");
        this.$router.push('home');
      });



      // TODO: call api with roomId
      // call api to join room, returns userId
      // var canJoin = true;
      // if (canJoin) {
      //   this.userId = 'newUserId';
      //   this.currentRoomComponent = 'RoomLobby';
      // } else {
      //   this.$router.push('home');
      // }
    },
    changeRoomComponent(newRoomComponent) {
      this.currentRoomComponent = newRoomComponent;
    },
  },
};  
</script>

<style>

</style>
