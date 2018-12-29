<template>
  <div class="room">
    This is a room with id {{ $route.params.roomId }}.
  </div>
</template>

<script>
export default {
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
      // call api with roomId
    },
  },
};  
</script>

<style>

</style>
