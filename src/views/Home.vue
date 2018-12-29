<template>
  <div class="home container">
    <div class="item">
      <h2 class="title">Koup</h2>
    </div>
    <div class="item">
      <HomeMenu/>
    </div>
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
  height: 50vh;
  background-color: blue;
  color: green;
}

.title {
  margin: 0;
}

.container {
  background-color: pink;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.item {
  margin: 0.5em;
  color: red;
}
</style>
