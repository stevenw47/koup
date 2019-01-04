<template>
  <div class="home container">
    <div class="title-container">
      <h1 class="title">Koup</h1>
    </div>
    <div class="menu-container">
      <HomeMenu/>
    </div>
    <!-- <div class="item">
      <h1 class="title">Koup</h1>
    </div>
    <div class="item">
      <HomeMenu/>
    </div> -->
  </div>
</template>

<script>
// @ is an alias to /src
import HomeMenu from '@/components/HomeMenu.vue';

export default {
  // name: 'home',
  components: {
    HomeMenu,
  },
};
</script>

<style scoped>
.home {
  height: 100vh;
}

.title {
  font-size: 50px;
  margin: 0;
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.title-container, .menu-container {
  margin: 0.5em;
  display: flex;
  justify-content: center;
}

/*.item {
  margin: 0.5em;
}*/
</style>
