<template>
  <div class="room-game">
    Room Game with roomId {{ roomId }} and userId {{ userId }}.
  </div>
</template>

<script>
export default {
  props: ['roomId', 'userId'],
};
</script>

<style scoped>
</style>