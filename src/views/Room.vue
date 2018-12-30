<template>
  <div class="room">
    This is a room with id {{ roomId }}.
    <component v-bind:is="currentRoomComponent" v-bind:roomId="roomId" v-bind:userId="userId"></component>
  </div>
</template>

<script>
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
    }
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
      // TODO: call api with roomId
      // call api to join room, returns userId
      canJoin = true;
      if(canJoin) {
        this.userId = 'newUserId';
        this.currentRoomComponent = 'RoomLobby';
      } else {
        this.$router.push('home');
      }
    },
  },
};  
</script>

<style>

</style>
