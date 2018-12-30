<template>
  <div class="room-lobby">
    Room Lobby with roomId {{ roomId }} and userId {{ userId }}.
  </div>
</template>

<script>
export default {
  props: ['roomId', 'userId'],
};
</script>

<style scoped>
</style>