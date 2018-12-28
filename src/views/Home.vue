<template>
  <div class="home">
    <h2>Koup</h2>
    <home-menu></home-menu>
  </div>
</template>

<script>
// @ is an alias to /src
import HomeMenu from '@/components/HomeMenu.vue';

export default {
  name: 'home',
  components: {
    HomeMenu,
  },
};
</script>
