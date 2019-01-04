<template>
  <div class="room">
    <component
      v-if="currentRoomComponent != ''"
      v-bind:is="currentRoomComponent"
      v-bind:roomId="roomId"
      v-bind:userId="userId"
      v-on:change-room-component="changeRoomComponent"
    />
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
      // react to route changes
      this.currentRoomComponent == '';
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
        this.$router.push({ name: 'home' });
      });
    },
    changeRoomComponent(newRoomComponent) {
      this.currentRoomComponent = newRoomComponent;
    },
  },
};  
</script>

<style scoped>
.room {
  height: 100vh;
}
</style>
